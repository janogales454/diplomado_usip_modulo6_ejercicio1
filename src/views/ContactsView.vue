<template>
    <div class="home">
        <Contacts msg="Lista de contactos"/>
    </div>
    
    <div class="mb-3">
        <div>
            <div class="input-group">
                <span class="input-group-text" id="basic-addon3">Filtrar por:</span>
                <input type="search" v-model="filterName" placeholder="Nombre" class="form-control">
            </div>
            <div class="input-group">
                <span class="input-group-text" id="basic-addon3">Filtrar por:</span>
                <input type="search" v-model="filterMail" placeholder="Correo" class="form-control">
            </div>
        </div>
    </div>
    <div class="mb-3">
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Correo</th>
                    <th scope="col">Direccion</th>
                    <th scope="col">Telefono</th>
                    <th scope="col">Pais</th>
                    <th scope="col">Ciudad</th>
                    <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(contact, index) in getContacts" :key="contact.id">
                    <th scope="row">{{ contact.id }}</th>
                    <td>{{ contact.name }}</td>
                    <td>{{ contact.email }}</td>
                    <td>{{ contact.address }}</td>
                    <td>{{ contact.phone }}</td>
                    <td>{{ contact.country }}</td>
                    <td>{{ contact.city }}</td>
                    <td>
                        <div class="btn-group" role="group">
                            <button type="button" class="btn btn-primary" @click="showEditContact(index)" title="Editar Contacto"><span class="material-symbols-outlined">edit</span></button>
                            <button type="button" class="btn btn-danger" @click="deleteContact(index)" title="Eliminar Contacto"><span class="material-symbols-outlined">delete</span></button>
                        </div>
                    </td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td colspan="8">
                        <button type="button" class="btn btn-primary" @click="showCreateContact()" title="Crear Nuevo Contacto"><span class="material-symbols-outlined">add</span></button>
                    </td>
                </tr>
            </tfoot>
        </table>
    
        
    </div>
    <div class="modal fade" id="contactModal" tabindex="-1" aria-labelledby="contactModalLabel" aria-hidden="true" ref="modalRef">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="contactModalLabel">Detalles de Contacto</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Cerrar"></button>
                </div>
                <div class="modal-body">
                    <EditContact v-if="modalMode ==2 && selectedContact" :contact="selectedContact" @update="editContact" @cancelar="closeModal" /> 
                    <NewContact v-if="modalMode == 1" @created="createNewContact($event)"></NewContact>    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import Contacts from '@/components/Contacts.vue';
import NewContact from '@/components/NewContact.vue';
import EditContact from '@/components/EditContact.vue';

export default {
    name: 'ContactsView',
    components: {
        Contacts,
        NewContact,
        EditContact
    },
    data() {
        return {
            contactLst: [
                {
                    "id": 1,
                    "name": "Alice Johnson",
                    "email": "alice.johnson@example.com",
                    "address": "123 Maple Street",
                    "phone": "123-456-7890",
                    "country": "USA",
                    "city": "New York"
                },
                {
                    "id": 2,
                    "name": "Bob Smith",
                    "email": "bob.smith@example.com",
                    "address": "456 Oak Avenue",
                    "phone": "987-654-3210",
                    "country": "Canada",
                    "city": "Toronto"
                },
                {
                    "id": 3,
                    "name": "Carol White",
                    "email": "carol.white@example.com",
                    "address": "789 Pine Road",
                    "phone": "555-123-4567",
                    "country": "UK",
                    "city": "London"
                },
                {
                    "id": 4,
                    "name": "David Brown",
                    "email": "david.brown@example.com",
                    "address": "321 Elm Street",
                    "phone": "444-555-6666",
                    "country": "Australia",
                    "city": "Sydney"
                },
                {
                    "id": 5,
                    "name": "Emily Davis",
                    "email": "emily.davis@example.com",
                    "address": "654 Spruce Lane",
                    "phone": "333-444-5555",
                    "country": "USA",
                    "city": "Los Angeles"
                }
            ],
            selectedContact: null,
            selectedIndex: 0,
            modalContactInst: null,
            modalMode:1,
            filterName:"",
            filterMail:"",
        };
    },
    computed: {
        getContacts() {
            let result = [...this.contactLst];
            result = result.filter((contact) => {
                return this.compareStrings(contact.name,this.filterName);
            });
            result = result.filter((contact) => {
                return this.compareStrings(contact.email,this.filterMail);
            });
            return result;
        }
    },
    methods:{
        compareStrings(value, input){
            const _value = value || "";
            const _input = input || "";
            return _value.toLowerCase().includes(_input.toLocaleLowerCase());
        },
        openModal(){
            if (this.modalContactInst) {
                this.modalContactInst.show();
            } else {
                console.error('La ventana no fue inicializada');
            }
        },
        closeModal() {
            if (this.modalContactInst) {
                this.modalContactInst.hide();
            }
        },
        showCreateContact() {
            this.modalMode=1;
            this.openModal();
        },
        showEditContact(index) {
            this.modalMode=2;
            this.selectedContact = null;
            this.selectedIndex = index;
            setTimeout(() => {
                this.selectedContact = { ...this.contactLst[index] };
                this.openModal();
            });

        },
        createNewContact($event){
            const maxId = Math.max(...this.contactLst.map(contact => contact.id));
            $event['id'] = maxId + 1;
            this.contactLst.push($event);
            this.closeModal();
        },
        editContact(editedContact) {
            this.contactLst[this.selectedIndex] = editedContact;
            this.closeModal();
        },
        deleteContact(index) {
            if (confirm('¿Está seguro de eliminar este contacto?')) {
                this.contactLst.splice(index, 1);
            }
        },
    },
    mounted() {
        this.$nextTick(() => {
            if (this.$refs.modalRef) {
                this.modalContactInst = new bootstrap.Modal(this.$refs.modalRef);
            } else {
                console.error('No se pudo inicializar el Modal.');
            }
        });
    }
}
</script>

<style lang="scss">
    .table>:not(caption)>*>*{
        border-color: #42b983;
    }
    .table>thead:not(caption)>*>*, .table>tfoot:not(caption)>*>*{
        background-color: #42b983;
        color:#ffffff;
    }
</style>
