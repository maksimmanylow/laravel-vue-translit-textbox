<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Транслит текстбокс</div>

                    <div class="card-body">
                        <div class="form-group">
                            <textarea class="form-control" v-model="text" rows="10" cols="40"/>
                        </div>
                        <div>Символов введено: {{charCount}}</div>
                        <button @click="transliterate">Транслитерировать</button>
                        <button @click="untransliterate">Растранслитерировать</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import C from '../constants';

    export default {
        data: () => ({
            text: '',
            }),
        computed: {
            charCount: function () {return this.text.length;},
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
            }
        }
    }
</script>
