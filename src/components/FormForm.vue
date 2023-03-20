<template>
    <form class="form" action="/">
        <div class="form__inputgroup">
            <label for="name" class="label">name</label>
            <ValidationProvider class="validation-prov" name="fio" :rules="{ regex: /^[A-Za-z]$/ }">
                <div slot-scope="{ errors }">
                    <input type="text" class="input" id="name" v-model="name">
                    <p>{{ errors[0] }}</p>
                </div>
            </ValidationProvider>
        </div>
        <div class="form__inputgroup">
            <label for="phone" class="label">phone</label>
            <input ref="phone" v-model="phone" type="text" maxlength="22" class="input" id="phone" @focus="e => focusHandler(e)">
        </div>
        <div class="form__inputgroup">
            <label for="temp" class="label">temp</label>
            <FormRanger class="form-ranger" text1="hot" text2="cold" v-model="temp"/>
        </div>
        <div class="form__inputgroup">
            <label for="comment" class="label">comment</label>
            <ValidationProvider class="validation-prov" name="fio" :rules="{ regex: /^[A-Za-z]$/ }">
                <div slot-scope="{ errors }">
                    <input type="text" class="input" id="comment" v-model="comment">
                    <p>{{ errors[0] }}</p>
                </div>
            </ValidationProvider>
        </div>
        <div class="form__buttongroup">
            <button class="button">call me</button>
            <p class="privacy">By pressing “Send” button I agree with <a href="#" class="privacylink">Privacy Policy</a></p>
        </div>
    </form>
</template>

<script>
import FormRanger from '@/components/FormRanger.vue'
import { extend } from 'vee-validate'
import { regex } from 'vee-validate/dist/rules'

extend('regex', {
    ...regex,
    message: 'invalid format'
})

export default {
    name: 'FormForm',
    data() {
        return {
            name: '',
            phone: '',
            temp: 20,
            comment: '',
        }
    },
    methods: {
        focusHandler(e) {
            if(this.phone === '') {
                this.phone = '+7 ('
                setTimeout(() =>{
                    e.target.selectionStart = 4
                    e.target.selectionEnd = 4
                    this.$refs.phone.oninput = this.inputHandler.bind(this)
                })
            }
        },
        inputHandler(e) {
            if(!e.data.match(/\d/)) {
                this.phone = this.phone.slice(0, -1)
            }
            if(e.inputType !== 'deleteContentBackward' && e.inputType !== 'deleteContentForward') {
                if(this.phone.length === 7) {
                    this.phone = this.phone + ') '
                }
                else if(this.phone.length === 12 || this.phone.length === 17) {
                    this.phone = this.phone + ' - '
                }
            }
        }
    },
    components: {
        FormRanger
    }
}
</script>