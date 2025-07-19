<template>
    <form @submit.prevent="emitirDatosEditados" novalidate>
        <div class="container mt-4">
            <h2 class="mb-4">Editar Contacto</h2>
            <!-- Nombre -->
            <input v-model="editedContact.name" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.name.$error}" placeholder="Nombre"/>
            <div class="invalid-feedback">Nombre es obligatorio</div>
            <!-- Correo -->
            <input v-model="editedContact.email" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.email.$error}" placeholder="Correo" />
            <div class="invalid-feedback">Correo es obligatorio</div>
            <!-- Direccion -->
            <input v-model="editedContact.address" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.address.$error}" placeholder="Direccion" />
            <div class="invalid-feedback">Direccion es obligatoria</div>
            <!-- Telefono -->
            <input v-model="editedContact.phone" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.phone.$error}" placeholder="Telefono" />
            <div class="invalid-feedback">Telefono es obligatorio</div>
            <!-- Pais -->
            <input v-model="editedContact.country" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.country.$error}" placeholder="Pais" />
            <div class="invalid-feedback">Pais es obligatorio</div>
            <!-- Ciudad -->
            <input v-model="editedContact.city" type="text" class="form-control" :class="{'is-invalid': v$.editedContact.city.$error}" placeholder="Ciudad" />
            <div class="invalid-feedback">Ciudad es obligatoria</div>
            <!-- Acciones -->
            <button type="submit" class="btn btn-primary">Guardar Contacto</button>
        </div>
    </form>
</template>

<script>
    import { reactive } from 'vue'
    import useVuelidate from '@vuelidate/core'
    import { required, } from '@vuelidate/validators'

    export default {
        name: 'EditContact',
        props: {
            contact: {
                type: Object,
                required: true
            }
        },
        data() {
            return {
                editedContact: reactive({ ...this.contact }),
                v$: null
            }
        },
        created() {
            const rules = {
                editedContact: {
                    name: { required },
                    email: { required },
                    address: { required },
                    phone: { required, },
                    country: { required, },
                    city: { required },
                }
            }
            this.v$ = useVuelidate(rules, { editedContact: this.editedContact });
        },
        methods: {
            async emitirDatosEditados() {
                const isValid = await this.v$.$validate()
                if (!isValid) {
                    alert('Por favor complete correctamente el formulario.')
                    return
                }
                this.$emit('update', this.editedContact)
            }
        }
    }
</script>

<style>
    form input{
        margin-bottom: 10px;
    }
</style>
  