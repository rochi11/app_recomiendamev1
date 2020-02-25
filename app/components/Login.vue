<template>
<Page actionBarHidden="true">
        <!-- <FlexboxLayout class="page"> -->
            <StackLayout class="form loginpersonalizado">
                <Image class="logo" src="~/images/promociones.jpg"></Image>
                <GridLayout rows="auto, auto, auto" class="formulario">
                    <StackLayout row="0" class="input-field">
                        <TextField class="input login-label" hint="RUC" :isEnabled="!processing"
                            autocorrect="false"
                            autocapitalizationType="none" v-model="user.ruc"
                            ></TextField>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>

                    <StackLayout row="1" class="input-field">
                        <TextField class="input login-label" :isEnabled="!processing"
                            autocorrect="false" autocapitalizationType="none"
                            hint="Usuario" v-model="user.usuario"
                           ></TextField>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>

                    <StackLayout row="2" class="input-field login-label">
                        <TextField class="input" ref="confirmPassword" :isEnabled="!processing"
                            hint="Clave" secure="true" v-model="user.clave"
                            returnKeyType="done"></TextField>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>

                    <ActivityIndicator rowSpan="3" :busy="processing"></ActivityIndicator>
                </GridLayout>

                <Button :text="isLoggingIn ? 'Ingresar' : 'Sign Up'" :isEnabled="!processing"
                    @tap="submit" class="btn btn-primary m-t-20 ingresar"></Button>
                <Label *v-show="isLoggingIn" text="Olvidaste tu clave?"
                    class="login-label" @tap="forgotPassword()"></Label>
            </StackLayout>
        <!-- </FlexboxLayout> -->
    </Page>
</template>

<script>
import axios from "axios";
import Main from "./Main";
const config = {
       headers: {
            'Content-type': 'application/json; charset=utf-8',
            'Access-Control-Allow-Origin':'*',
            'Access-Control-Allow-Methods': 'POST',
            'Access-Control-Allow-Headers':'*',
            'cache-control': 'no-cache'
          }
}
    export default {
        data(){
            return{
                isLoggingIn: true,
                processing: false,
                user: {
                    ruc: "20434178780",
                    usuario: "Administrador",
                    clave: "123456"
                },
                respuestas: [
                    {name:'Loki',ape:'feo'},
                    {name:'Fredo',ape:'feo'},
                    {name:'Mayquis',ape:'feo'},
                    {name:'Martin',ape:'feo'}
                    ],
                bandera: '',
                respuesta: []
            }
        },
        methods:{
            toggleForm() {
                this.isLoggingIn = !this.isLoggingIn;
            },
            submit() {   
                this.processing = true;
                this.isBusy = true,
                this.ingresarLogin = true,               
                axios
                .get('http://10.0.2.2:3000/api/usuario/listar', 
                {
                    headers: {
                        'Content-type': 'application/json; charset=utf-8',
                        'Access-Control-Allow-Origin':'*',
                        'Access-Control-Allow-Methods': 'GET',
                        'Access-Control-Allow-Headers':'*',
                        'cache-control': 'no-cache'
                    }
                })
                .then(response => (
                    this.respuesta = response.data,
                    this.isBusy = false,
                    this.ingresarLogin = false,
                    this.alert(this.respuesta),
                    this.$navigateTo(Main, { clearHistory: true })
                ))
                .catch(error => (
                    this.isBusy = false,
                    this.ingresarLogin = false,
                    this.$navigateTo(Main, { clearHistory: true })
                ));   
            },
            login() {
                this.$backendService
                    .login(this.user)
                    .then(() => {
                        this.processing = false;
                        this.$navigateTo(Home, { clearHistory: true });
                    })
                    .catch(() => {
                        this.processing = false;
                        this.alert(
                            "Unfortunately we could not find your account."
                        );
                    });
            },

            register() {
                if (this.user.password != this.user.confirmPassword) {
                    this.alert("Your passwords do not match.");
                    this.processing = false;
                    return;
                }

                this.$backendService
                    .register(this.user)
                    .then(() => {
                        this.processing = false;
                        this.alert(
                            "Your account was successfully created.");
                        this.isLoggingIn = true;
                    })
                    .catch(() => {
                        this.processing = false;
                        this.alert(
                            "Unfortunately we were unable to create your account."
                        );
                    });
            },

            forgotPassword() {
                prompt({
                    title: "Olvedaste la Clave",
                    message: "Ingresa el correo electronico con el que registraste tu app, para resetear la clave",
                    inputType: "email",
                    defaultText: "",
                    okButtonText: "Ok",
                    cancelButtonText: "Cancelar"
                }).then(data => {
                    if (data.result) {
                        this.$backendService
                            .resetPassword(data.text.trim())
                            .then(() => {
                                this.alert(
                                    "Your password was successfully reset. Please check your email for instructions on choosing a new password."
                                );
                            })
                            .catch(() => {
                                this.alert(
                                    "Unfortunately, an error occurred resetting your password."
                                );
                            });
                    }
                });
            },

            focusPassword() {
                this.$refs.password.nativeView.focus();
            },
            focusConfirmPassword() {
                if (!this.isLoggingIn) {
                    this.$refs.confirmPassword.nativeView.focus();
                }
            },

            alert(message) {
                return alert({
                    title: "APP NAME",
                    okButtonText: "OK",
                    message: message
                });
            }
        }
    }
</script>

<style>
    .logo{
        margin-top: 5%;
        margin-bottom: 25%;
        width: 50%;
    }
    .loginpersonalizado{
        margin-left: 10%;
        margin-right: 10%;
    }
    .login-label{
        color: #6E6E6E;
    }
    .formulario{
        margin-top: -120x;
    }
    .ingresar{
        width: 100%;
    }
</style>


