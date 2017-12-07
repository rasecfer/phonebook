<template>
    <div>
        <nav class="panel column is-offset-2 is-8">
            <div class="panel-heading">
            <span class="column">
                Vuejs Phonebook
                <i class="fa fa-refresh fa-spin fa-2x fa-fw is-pulled-right" v-if="loading"></i>
                <a class="button is-success is-pulled-right" @click="showAdd">
                    <span class="icon is-small">
                      <i class="fa fa-address-book"></i>
                    </span>
                    <span>Add New</span>
                </a>
            </span>
            </div>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="search" v-model="searchKeyword">
                    <span class="icon is-small is-left">
                    <i class="fa fa-search"></i>
                </span>
                </p>
            </div>
            <div class="panel-block" v-for="(contact, index) in tempContacts">
                <span class="column is-9">
                    {{ contact.name }}
                </span>
                <span class="panel-icon column is-1" style="cursor: pointer;">
                  <i class="has-text-danger fa fa-trash" @click="deleteContact(index, contact.id)"></i>
                </span>
                <span class="panel-icon column is-1" style="cursor: pointer;">
                  <i class="has-text-info fa fa-edit" @click="showUpdate(index)"></i>
                </span>
                <span class="panel-icon column is-1" style="cursor: pointer;">
                  <i class="has-text-primary fa fa-eye" @click="showShow(index)"></i>
                </span>
            </div>
        </nav>

        <Add :openModal="active" @closeRequest="close"></Add>
        <Show :openModal="showActive" @closeRequest="close"></Show>
        <Update :openModal="updateActive" @closeRequest="close"></Update>

    </div>
</template>

<script>
    import Add from './Add.vue'
    import Show from './Show.vue'
    import Update from './Update.vue'
    export default {
        data() {
            return {
                active: '',
                showActive: '',
                updateActive: '',
                contacts: {},
                errors: {},
                loading: false,
                searchKeyword: '',
                tempContacts: ''
            }
        },
        watch: {
            searchKeyword() {
                if(this.searchKeyword.length > 0) {
                    this.tempContacts = this.contacts.filter((item) => {
                        return Object.keys(item).some((key) => {
                            let string = String(item[key])
                            return string.toLowerCase().indexOf(this.searchKeyword.toLowerCase()) > -1
                        });
                    });
                } else {
                    this.tempContacts = this.contacts
                }
            }
        },
        mounted() {
            axios.post('/getData')
                .then((response) => this.contacts = this.tempContacts = response.data)
                .catch((error) => this.errors = error.response.data.errors)
        },
        components: {
            Add,
            Show,
            Update
        },
        methods: {
            showAdd() {
                this.active = 'is-active'
            },
            showShow(index) {
                this.$children[1].list = this.tempContacts[index]
                this.showActive = 'is-active'
            },
            showUpdate(index) {
                this.$children[2].list = this.tempContacts[index]
                this.updateActive = 'is-active'
            },
            close() {
                this.active = ''
                this.showActive = ''
                this.updateActive = ''
            },
            deleteContact(index, id) {
                if(confirm('Are you sure?')) {
                    axios.delete(`/phonebook/${id}`)
                        .then((response) => this.contacts.splice(index, 1))
                        .catch((error) => this.errors = error.response.data.errors)
                }
            }
        }
    }
</script>