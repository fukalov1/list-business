<template>
    <div>
        <span class="h5">
         <slot>
            Название формы
         </slot>
        </span>
        <table class="table">
            <tr>
                <td>
                    <label>Время</label>
                    <masked-input class="form-control text-center" type="text" v-model="data.time" mask="11:11" placeholder="чч:мм"/>
                </td>
            </tr>
            <tr>
                <td>
                    <label>Наименование</label>
                    <input class="form-control" type="text" v-model="data.name">
                </td>
            </tr>
            <tr>
                <td>
                    <label>Подробности</label>
                    <textarea class="form-control" v-model="data.text">

                    </textarea>
                </td>
            </tr>
            <tr>
                <td>
                    <label>Выполнено</label>
                    <input type="checkbox" v-model="data.exec" class="form-control">
                </td>
            </tr>
            <tr>
                <td class="text-center">
                    <div class="row">
                        <div class="col-xl-4 col-lg-4 col-md-4 col-4">
                            <button type="button" class="btn btn-primary" @click="$emit('save-data', data)">
                                сохранить
                            </button>
                        </div>
                        <div class="col-xl-4 col-lg-4 col-md-4 col-4">
                            <button type="button" class="btn btn-info" @click="$emit('set-cancel')">
                                отменить
                            </button>
                        </div>
                        <div class="col-xl-4 col-lg-4 col-md-4 col-4">
                            <button type="button" class="btn btn-danger" @click="$emit('confirm-delete', data)">
                                удалить
                            </button>
                        </div>
                    </div>


                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    import MaskedInput from 'vue-masked-input'

    export default {
        name: 'form-business',
        components: {
            MaskedInput
        },
        props: {
            oneBusiness: {
                type: Object,
                required: true
            },
            currentItem: {
                type: Number,
                default: null
            }
        },
        data() {
            return {
                data: this.oneBusiness
            }
        },
        methods: {

        }
    }
</script>