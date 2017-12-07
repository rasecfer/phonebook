<template>
    <div class="modal" :class="openModal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Show Contact</p>
                <button class="delete" aria-label="close" @click="closeModal"></button>
            </header>
            <section class="modal-card-body">
                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" type="text" placeholder="Name" v-model="list.name" readonly>
                        <span class="icon is-small is-left">
                          <i class="fa fa-user"></i>
                        </span>
                    </p>
                </div>

                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" type="number" placeholder="Phone #" v-model="list.phone" readonly>
                        <span class="icon is-small is-left">
                          <i class="fa fa-phone"></i>
                        </span>
                    </p>
                </div>

                <div class="field">
                    <p class="control has-icons-left has-icons-right">
                        <input class="input" type="email" placeholder="Email" v-model="list.email" readonly>
                        <span class="icon is-small is-left">
                          <i class="fa fa-envelope"></i>
                        </span>
                    </p>
                </div>

            </section>
            <footer class="modal-card-foot">
                <button class="button" @click="closeModal">Close</button>
            </footer>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                list: {}
            }
        },
        props: ['openModal'],
        methods: {
            closeModal() {
                this.$emit('closeRequest')
            }
        }
    }
</script>