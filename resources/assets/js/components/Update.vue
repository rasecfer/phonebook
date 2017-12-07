<template>
    <div class="modal" :class="openModal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Update {{ list.name }}</p>
                <button class="delete" aria-label="close" @click="closeModal"></button>
            </header>
            <section class="modal-card-body">
                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" :class="{'is-danger':errors.name}" type="text" placeholder="Name" v-model="list.name">
                        <span class="icon is-small is-left">
                          <i class="fa fa-user"></i>
                        </span>
                        <span class="icon is-small is-right">
                          <i class="fa fa-check"></i>
                        </span>
                    </p>
                    <small class="has-text-danger" v-if="errors.name">{{ errors.name[0] }}</small>
                </div>

                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" :class="{'is-danger':errors.phone}" type="number" placeholder="Phone #" v-model="list.phone">
                        <span class="icon is-small is-left">
                          <i class="fa fa-phone"></i>
                        </span>
                        <span class="icon is-small is-right">
                          <i class="fa fa-check"></i>
                        </span>
                    </p>
                    <small class="has-text-danger" v-if="errors.phone">{{ errors.phone[0] }}</small>
                </div>

                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" :class="{'is-danger':errors.email}" type="email" placeholder="Email" v-model="list.email">
                        <span class="icon is-small is-left">
                          <i class="fa fa-envelope"></i>
                        </span>
                        <span class="icon is-small is-right">
                          <i class="fa fa-check"></i>
                        </span>
                    </p>
                    <small class="has-text-danger" v-if="errors.email">{{ errors.email[0] }}</small>
                </div>

            </section>
            <footer class="modal-card-foot">
                <button class="button is-success" @click="update">Update</button>
                <button class="button" @click="closeModal">Cancel</button>
            </footer>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                list: {},
                errors: {}
            }
        },
        props: ['openModal'],
        methods: {
            closeModal() {
                this.$emit('closeRequest')
            },
            update() {
                axios.patch(`/phonebook/${this.list.id}`, this.$data.list)
                    .then((response) => this.closeModal())
                    .catch((error) => this.errors = error.response.data.errors)
            }
        }
    }
</script>