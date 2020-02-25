<template>
	<page actionBarHidden="true" backgroundSpanUnderStatusBar="true">
		<!-- @loaded="onLoaded" -->
		<GridLayout rows="*" height="100%" backgroundColor="#E2E2E2">
			<RadSideDrawer ref="drawer">
                <StackLayout ~drawerContent backgroundColor="#fff">
                    <StackLayout height="150" style="text-align: center; vertical-align: center;">
                        <Image class="logo" src="~/images/bar_code.png"></Image>
                    </StackLayout>
					<GridLayout columns="20,auto" rows="*,*,*,6*,*" class="m-l-8">
						<Label row="0" col="0" class="fa" :text="'fa-file' | fonticon" verticalAlignment="center"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
                        <Label row="0" col="1" text="Registro" class="menubar m-t-8 m-b-8" padding="5" @tap="tapVoucher(1)"/>
						<Label row="1" col="0" class="fa" :text="'fa-file' | fonticon" verticalAlignment="center"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
                        <Label row="1" col="1" text="Configuracion" class="menubar m-t-8 m-b-8" padding="5" @tap="tapVoucher(2)"/>
						<Label row="2" col="0" class="fa" :text="'fa-cogs' | fonticon" verticalAlignment="center"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
                        <Label row="2" col="1" text="Favoritos" class="menubar m-t-8 m-b-8" padding="5" @tap="tapConfiguracion"/>
						<Label row="4" col="0" class="fa" :text="'fa-times-circle' | fonticon"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
						<Label row="4" col="1" text="Cerrar Sesion" @tap="logoutUser" />
                    </GridLayout>

                    <!-- <GridLayout columns="20,auto" rows="auto, auto, auto" class="m-l-8">
						<Label row="0" col="0" class="fa" :text="'fa-bars' | fonticon" verticalAlignment="center"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
                        <Label row="0" col="1" text="Ingresa el cupon" padding="10" />
						<Label row="1" col="0" class="fa" :text="'fa-bars' | fonticon" verticalAlignment="center"
                    		horizontalAlignment="left" iosOverflowSafeArea="false" />
                        <Label row="1" col="1" text="Editar perfil" padding="10" />
                    </GridLayout>
                    <Label text="Cerrar Sesion" color="black" padding="10" style="horizontal-align: center"
                        @tap="onCloseDrawerTap" /> -->
                </StackLayout>
				<StackLayout ~mainContent>
					<GridLayout row="0" ref="navStatusBar" class="navStatusBar" backgroundColor="#bd2122" verticalAlignment="top" height="40"
					width="100%" rows="auto" columns="*,auto,auto,auto">
						<Label :text="'fa-bars' | fonticon" @tap="onOpenDrawerTap" class="fa button-bars"
							row="0"	textWrap="true" />
						<Image col="1" row="0" @tap="search" horizontalAlignment="right" class="status-img"
							src="~/assets/images/search.png" />
						<Image col="2" row="0" @tap="bell" horizontalAlignment="right" class="status-img"
							src="~/assets/images/bell.png" />
						<Image horizontalAlignment="right" stretch="aspectFill" col="3"
							row="0" class="status-profile" src="~/assets/images/me.jpg" />
					</GridLayout>

					<GridLayout  row="1" ref="navTab" class="navTab" verticalAlignment="top" height="50"
							width="100%"  rows="auto" columns="auto,auto,auto">

							<GridLayout class="tabview" :class="selectedTabview==0?'active':''"
									@tap="popular" rows="*,auto" cols="auto" col="0" row="0"
									width="33%">
									<Image v-show="selectedTabview==0" row="0" class="navIcon"
											:src="selectedTabview==0?'~/assets/images/popular.png':''"/>
									<Label :class="selectedTabview==0?'active':''" row="1"
											text="POPULAR" class="tabviewText"></Label>
							</GridLayout>
							<GridLayout class="tabview" :class="selectedTabview==1?'active':''"
									@tap="showCategory" rows="*,auto" cols="auto" col="1" row="0"
									width="34%">
									<Image v-show="selectedTabview == 1" row="0" class="navIcon"
											:src="selectedTabview==1?'~/assets/images/category.png':''"/>
									<Label :class="selectedTabview==1?'active':''" row="1"
											text="CATEGORY" class="tabviewText"></Label>
							</GridLayout>
							<GridLayout class="tabview" :class="selectedTabview==2?'active':''"
									@tap="showPromos" rows="*,auto" cols="auto" col="2" row="0"
									width="33%">
									<Image v-show="selectedTabview == 2" row="0" class="navIcon"
											:src="selectedTabview==2?'~/assets/images/category.png':''"/>
									<Label :class="selectedTabview==2?'active':''" row="1"
											text="PROMOS" class="tabviewText"></Label>
							</GridLayout>
					</GridLayout>

					<GridLayout v-show="selectedTabview == 0" row="2" width="100%" backgroundColor="white">
						<ListView ref="listview" separatorColor="transparent" for="(item, index) in items" :key="index">
							<v-template>
								<item :item="item" @clicked="showItem(item)" />
							</v-template>
						</ListView>
					</GridLayout>

					<GridLayout v-show="selectedTabview == 1" row="2" width="100%" backgroundColor="white" rows="60, *" columns="*">
						<AbsoluteLayout backgroundColor="#fff" row="0" col="0">
							<Label text="" left="10" top="20" width="95%" height="20" class="barcodefranja" backgroundColor="#EC0000"/>
							<Label @tap="scanQrCode" left="200" top="5" class="fa barcode" :text="'fa-qrcode' | fonticon" verticalAlignment="center"
							horizontalAlignment="center" iosOverflowSafeArea="false" />
						</AbsoluteLayout>
						<ListView ref="listview" row="1" col="0" separatorColor="transparent" for="(item, index) in itemsCategory" :key="index">
							<v-template>
								<Category :item="item"> </Category>
							</v-template>
						</ListView>
					</GridLayout>

					<GridLayout v-show="selectedTabview == 2" row="2" width="100%" backgroundColor="white" rows="*" columns="*">
						<Label @tap="scanQrCode" class="fa barcode" col="0" :text="'fa-qrcode' | fonticon" verticalAlignment="center"
							horizontalAlignment="center" iosOverflowSafeArea="false" />
					</GridLayout>

					<navBottom row="3" />
				</StackLayout>
			</RadSideDrawer>
		</GridLayout>
</page>
</template>
<script>
	// import { SwissArmyKnife } from "nativescript-swiss-army-knife";
	import axios from 'axios'
	import { isIOS, isAndroid } from 'tns-core-modules/platform'
	import * as Camera from "nativescript-camera";
	import navBottom from "./custom/navBottom";
	import Item from "./custom/item";
	import Category from "./custom/category";
	import ItemDetails from "./ItemDetails";
	import { alert, confirm, prompt, login, action, inputType } from "tns-core-modules/ui/dialogs";
	const BarcodeScanner = require("nativescript-barcodescanner").BarcodeScanner;
    var barcodescanner = new BarcodeScanner();
	const gestures = require("ui/gestures");
	const app = require("application");

export default {
    name: 'Main',
	components: {
		navBottom,
		Item,
		Category
	},
	computed: {
		itemsCategory(){
			return this.category.slice().reverse();
		}
	},

	mounted () {
		// SwissArmyKnife.setAndroidStatusBarColor("#b51213");
	},
	data() {
		return {
			listaDescription: [],
			lastDelY: 0,
			headerCollapsed: false,
			selectedTab: 0,
			selectedTabview: 0,
			items: [{
				name: "Manila Ultimate Tombstone Burger",
				cover: "~/assets/images/food/burger640.jpg",
				images: [
						{src: "~/assets/images/food/burger/burger1.jpg"},
						{src: "~/assets/images/food/burger/burger2.jpg"},
						{src: "~/assets/images/food/burger/burger3.jpg"},
						{src: "~/assets/images/food/burger/burger4.jpg"},
						{src: "~/assets/images/food/burger/burger5.jpg"},
						{src: "~/assets/images/food/burger/burger6.jpg"}
					],
				category: "Burger",
				categoryTag: "#2D9CDB",
				price: "300.00",
				likes: 987,
				isLike: false,
				isFavorite: true,
				comments: 13,
				rating: "4.5",
				description: "a"
			},
			{
				name: "Quezon Chocolate Marble Pancake",
				cover: "~/assets/images/food/pancake640.jpg",
				images: [
					{src: "~/assets/images/food/pancake/pancake1.jpg"},
					{src: "~/assets/images/food/pancake/pancake2.jpg"},
					{src: "~/assets/images/food/pancake/pancake3.jpg"},
					{src: "~/assets/images/food/pancake/pancake4.jpg"},
					{src: "~/assets/images/food/pancake/pancake5.jpg"},
					{src: "~/assets/images/food/pancake/pancake6.jpg"}
				],
				category: "Pancake",
				categoryTag: "#e4ce0d",
				price: "230.00",
				likes: 891,
				isLike: true,
				isFavorite: true,
				comments: 7,
				rating: "4.0",
				description: "a"
			},
			{
				name: "Binondo Black Forest Cake",
				cover: "~/assets/images/food/cake640.jpg",
				images: [
					{src: "~/assets/images/food/cake/cake1.jpg"},
					{src: "~/assets/images/food/cake/cake2.jpg"},
					{src: "~/assets/images/food/cake/cake3.jpg"},
					{src: "~/assets/images/food/cake/cake4.jpg"}
				],
				category: "Cake",
				categoryTag: "#27AE60",
				price: "300.00",
				likes: 730,
				isLike: true,
				isFavorite: true,
				comments: 11,
				rating: "4.0",
				description: "a"
			},
			],
			category: [
			{
				cover: "~/assets/images/food/burger640.jpg",
				category: "BURGER",
				count: "13",
			},
			{
				cover: "~/assets/images/food/pancake640.jpg",
				category: "PANCAKE",
				count: "5",
			},
			{
				cover: "~/assets/images/food/cake640.jpg",
				category: "CAKE",
				count: "9",
			},
			{
				cover: "~/assets/images/food/beer640.jpg",
				category: "BEER",
				count: "7",
			},

			]
		};
	},
	methods: {
		logoutUser()
		{
			confirm({
				title: "Cerrar Sesion",
				message: "Seguro que desea cerrar la sesion?",
				okButtonText: "Aceptar",
				cancelButtonText: "Cancelar"
				}).then(result => {
					if(result){
						// this.$store.commit('logoutuser')
						this.$navigateTo(Login, { clearHistory: true })
					}
			});	
		},
		onOpenDrawerTap() {
            this.$refs.drawer.nativeView.showDrawer();
        },
        onCloseDrawerTap() {
            this.$refs.drawer.nativeView.closeDrawer();
        },
		scanQrCode() {
			Camera.requestCameraPermissions()
				.then(() => {
					barcodescanner.scan({
						formats: "EAN13",
						cancelLabel: "Close",
						cancelLabelBackgroundColor: "#FFFFFF",
						message: "Use the volume buttons for extra light",
						showFlipCameraButton: false,
						preferFrontCamera: false,
						showTorchButton: true,
						beepOnScan: true,
						torchOn: false,
						closeCallback: function () { console.log("Scanner closed"); },
						resultDisplayDuration: 500,
						orientation: "portrait",
						openSettingsIfPermissionWasPreviouslyDenied: true
					}).then((result) => {
						// console.log("Scan format : " + result.format);
						console.log("Scan text :   " + result.text);
						this.searchProductbyCodigo(result.text)
					},
						(error) => {
							console.log("No scan: " + error);
						}
					);

				}).catch(e => { console.log('Error requesting permission'); });
		},
		searchProductbyCodigo: function (value) {
			axios
			.post(this.$store.getters.getRoute + 'productos/buscar',{
				token: this.$store.getters.userToken,
				filtro: 'codegs1',
				p: value
			})
			.then(response => (
				this.listaDescription = response.data.response,
				this.updateValue('scanner')
			))
			.catch(error => (
				this.alert(error.response.data.msj, 'Atención')
			))
		},
		updateValue(val) {
			// setear la data que viene del api
			if(val === 'scanner'){
				// this.$store.commit('addproducto', this.listaDescription[0])
			}else{
			
			}
		},
		search(){
			console.log('search')
		},
		bell(){
			console.log('bell')
		},
		showItem(payload) {
			this.$navigateTo(ItemDetails,{
				props: {
					item: payload
				},
				animated: true,
				transition: {
					name: "slideTop",
					duration: 380,
					curve: "easeIn"
				}
			})
		},
		popular() {
			this.selectedTabview = 0;
		},
		showCategory() {
			this.selectedTabview = 1;
		},
		showPromos() {
			this.selectedTabview = 2;
		},
		home() {
			this.selectedTab = 0;
		},
		cart() {
			this.selectedTab = 1;
		},
		history() {
			this.selectedTab = 2;
		},
		about() {
			this.selectedTab = 3;
		}
	}
};
</script>

<style>
.tabview.active {
	border-bottom-color: white;
	border-bottom-width: 3;
	margin: 0;
	height: 50;
}
.tabviewText {
	margin-top: 15;
	margin-bottom: 5;
	font-size: 13;
	color: #d8d2d2;
	horizontal-align: center;
}
.tabviewText.active {
	margin-top: 0;
	margin-bottom: 5;
	font-weight: bold;
	color: white;
	vertical-align: center;
}
.navTab {
	background-color: #bd2122;
}
.navTabview {
	background-color: blue;
}
.status-img {
	margin-top: 4;
	margin-right: 20;
	width: 30;
	height: 30;
}
.status-profile {
	border-width: 1;
	border-color: #ffffff;
	background-color: #f1ebeb;
	border-radius: 50%;
	margin-top: 2;
	margin-right: 15;
	width: 30;
	height: 30;
}
.status-title {
	color: white;
	font-size: 18;
	margin-left: 15;
	margin-top: 8;
	horizontal-align: left;
	vertical-align: center;
}
.button-bars{
	margin-left: 30px;
	margin-top: 20px;
	font-size: 25px;
	color: #fff;
	padding: 10;
}
.button-menu{
	font-size: 25px;
	color: #000;
	padding: 10;
}
.barcodestyles{
	color: #003c5a;
	padding: 10;
	border-top-right-radius: 20;
	border-bottom-right-radius: 20;
	border-top-left-radius: 20;
	border-bottom-left-radius: 20;
}
.barcode{
	font-size: 25px;
	width: 120px;
	margin-top: 2;
	color: #fff;
	text-align: center;
	padding-top: 10%;
	border: none;
	height: 120px;
	border-radius: 60px;
	background-color: #B40000;
}
.barcodefranja{
	border: none;
	border-radius: 10px;
}
</style>