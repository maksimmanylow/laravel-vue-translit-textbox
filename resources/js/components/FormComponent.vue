<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">

                <div v-if="errors.length" class="alert alert-danger" role="alert">
                    <pre>{{errors}}</pre>
                </div>

                <div class="card card-default">
                    <div class="card-header text-center">Онлайн транслитерация</div>

                    <div class="card-body">
                        <div class="form-group">
                            <textarea class="form-control" v-model="text" rows="10" cols="40"/>
                        </div>
                        <div class="float-right">Символов введено <span class="badge badge-dark">{{charCount}}</span></div>
                        <div class="clearfix"></div>
                        <div class="float-right">Сообщений <span class="badge badge-dark">{{messageCount}}</span></div>
                        <div class="form-check">
                            <input @change="doSomething" v-model="doTransliterare" class="form-check-input" type="checkbox"  id="defaultCheck1">
                            <label class="form-check-label" for="defaultCheck1">
                                транслитерировать
                            </label>
                        </div>
                    </div>
                    <div class="card-footer text-right">
                            <button class="btn btn-primary" @click="save">Сохранить</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import C from '../constants';

    export default {
        data: () => ({
            text: '',
            doTransliterare: false,
            errors: []
            }),
        computed: {
            charCount: function () {return this.text.length;},
            messageCount: function () {
                if (!this.charCount)
                    return 0;
                if (this.doTransliterare) {
                    if (this.charCount > C.messageLengthStandards.whole.lat) {
                        return Math.ceil(this.charCount / C.messageLengthStandards.partial.lat);
                    } else return 1;
                } else {
                    if (this.charCount > C.messageLengthStandards.whole.ru) {
                        return Math.ceil(this.charCount / C.messageLengthStandards.partial.ru);
                    } else return 1;
                }
            }
        },
        methods: {
            transliterate: function(direction) {
                let regexp = null;
                let newText = this.text;
                C.TranslitTable.forEach(char => {
                    regexp = new RegExp(char.ru, 'g');
                    newText = newText.replace(regexp, char.lat);
                });
                this.text = newText;
            },
            untransliterate: function () {
                let regexp = null;
                let newText = this.text;
                C.TranslitTable.forEach(char => {
                    regexp = new RegExp(char.lat, 'g');
                    newText = newText.replace(regexp, char.ru);
                });
                this.text = newText;
            },
            doSomething: function () {
                if (this.doTransliterare) {
                    this.transliterate();
                } else {
                    this.untransliterate();
                }
            },
            save: function () {
                const data = {
                    text: this.text,
                    count: this.messageCount
                };
                axios.post(C.saveMessageURI, data)
                    .then(reposnse => {
                        console.info("Message saved");
                     } )
                    .catch(e => {this.errors.push(e)});
            }
        }
    }
</script>
