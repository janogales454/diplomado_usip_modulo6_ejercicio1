<template>
    <form @submit.prevent="submit" novalidate>
        <div class="container mt-4">
            <h2 class="mb-4">Registrar Nuevo Contacto</h2>
            <!-- Nombre -->
            <input v-model="contact.name" type="text" class="form-control" :class="{'is-invalid': v$.contact.name.$error}" placeholder="Nombre"/>
            <div class="invalid-feedback">Nombre es obligatorio</div>
            <!-- Correo -->
            <input v-model="contact.email" type="text" class="form-control" :class="{'is-invalid': v$.contact.email.$error}" placeholder="Correo" />
            <div class="invalid-feedback">Correo es obligatorio</div>
            <!-- Direccion -->
            <input v-model="contact.address" type="text" class="form-control" :class="{'is-invalid': v$.contact.address.$error}" placeholder="Direccion" />
            <div class="invalid-feedback">Direccion es obligatoria</div>
            <!-- Telefono -->
            <input v-model="contact.phone" type="text" class="form-control" :class="{'is-invalid': v$.contact.phone.$error}" placeholder="Telefono" />
            <div class="invalid-feedback">Telefono es obligatorio</div>
            <!-- Pais -->
            <input v-model="contact.country" type="text" class="form-control" :class="{'is-invalid': v$.contact.country.$error}" placeholder="Pais" />
            <div class="invalid-feedback">Pais es obligatorio</div>
            <!-- Ciudad -->
            <input v-model="contact.city" type="text" class="form-control" :class="{'is-invalid': v$.contact.city.$error}" placeholder="Ciudad" />
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
    name: 'NewContact',
    data() {
        return {
            title: ' NewContact',
            contact: reactive({
                id: 0,
                name: "",
                email: "",
                address: "",
                phone: "",
                country: "",
                city: ""
            }),
            v$: null
        }
    },
    created() {
        const rules = {
            contact: {
                name: { required },
                email: { required },
                address: { required },
                phone: { required, },
                country: { required, },
                city: { required },
            }
        }
        this.v$ = useVuelidate(rules, { contact: this.contact })
    },
    methods: {
        async submit() {
            const isValid = await this.v$.$validate()
            if (!isValid) {
                alert('Por favor complete correctamente el formulario.')
                return
            }
            this.$emit('created', { ...this.contact });
        }
    }
}
</script>

<style>
    form input{
        margin-bottom: 10px;
    }
</style>
