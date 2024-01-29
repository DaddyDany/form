<template>
  <div>
    <p v-if="errors.length">
        <b>Por favor, corrija el(los) siguiente(s) error(es):</b>
        <ul>
            <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
        </ul>
    </p>
    <b-form @submit="onSubmit">
      <b-form-group id="input-group-1" label="Nombre:" label-for="name">
        <b-form-input
          id="name"
          v-model="form.name"
          type="text"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Apellido Paterno:" label-for="lastname1">
        <b-form-input
          id="lastname1"
          v-model="form.lastname1"
          type="text"
        ></b-form-input>
      </b-form-group>

       <b-form-group id="input-group-2-5" label="Apellido Materno:" label-for="lastname2">
        <b-form-input
          id="lastname2"
          v-model="form.lastname2"
          type="text"
          
        ></b-form-input>
      </b-form-group>

      <h4>Dirección:</h4>
      <b-form-group id="input-group-3" label="CP:" label-for="cp">
        <b-form-input
          id="cp"
          v-model="form.cp"
          type="number"
          
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3-2" label="Calle:" label-for="street">
        <b-form-input
          id="street"
          v-model="form.street"
          type="text"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3-3" label="Número:" label-for="number">
        <b-form-input
          id="number"
          v-model="form.number"
          type="number"
          
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3-4" label="Ciudad:" label-for="city">
        <b-form-input
          id="city"
          v-model="form.city"
          type="text"
          
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-4"
        label="Fecha de nacimiento:"
        label-for="birthdate"
      >
        <b-form-input
          id="birthdate"
          v-model="form.birthdate"
          type="date"
          
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-5"
        label="Correo Electrónico:"
        label-for="email"
      >
        <b-form-input
          id="email"
          v-model="form.email"
          type="text"
          
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-6"
        label="Número telefónico:"
        label-for="phone"
      >
        <b-form-input
          id="phone"
          v-model="form.phone"
          type="number"
          
        ></b-form-input>
      </b-form-group>

      <b-form-file v-model="form.file" class="mt-3" plain accept="image/jpg"></b-form-file>
        <div class="mt-3">
        Archivo seleccionado: {{ form.file ? form.file.name : "" }}
      </div>

      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      errors: [],
      form: {
        name: "",
        lastname1: "",
        lastname2: "",
        cp: "",
        street: "",
        number: "",
        city: "",
        birthdate: "",
        email: "",
        phone: null,
        file: null,
      },
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      this.errors = [];
      let emailValue = this.validateEmail(this.form.email)
      let fullNameValue = this.validateFullname(this.form.name, this.form.lastname1);
      let birthdateValue = this.validateBirthdate(this.form.birthdate)
      let phoneValue = this.validatePhoneNumber(this.form.phone)
      let addressValue = this.validateAddress(this.form.cp, this.form.street, this.form.number, this.form.city)
      let fileValue = this.validateFileSize(this.form.file)
      if (fullNameValue && addressValue && emailValue && birthdateValue && phoneValue && fileValue) {
        alert("Datos Capturados con exito")
        return true;
      }
    },
    validateFullname: function(name, lastname){
      if (!name || !lastname) {
        this.errors.push('El nombre y apellido paterno son obligatorios.');
        return false;
      }
      return true;
    },
    validateAddress: function(cp, street, number, city){
        if (!cp) {
            this.errors.push('El codigo postal es obligatorio.')
            return false;
        } else if (cp.length != 5) {
            this.errors.push('El codigo postal debe ser de 5 caracteres.')
            return false;
        }else if (!street) {
            this.errors.push('La calle es obligatoria.')
            return false;
        }else if(!number) {
            this.errors.push('El numero es obligatorio.')
            return false;
        }else if(!city) {
            this.errors.push('La ciudad es obligatoria.')
            return false;
        }
        return true;
    },
    validateEmail: function(mail){
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if(!mail || (!emailRegex.test(mail))){
            this.errors.push("La dirección de correo electrónico no es válida.");
            return false;
        }
        return true;
    },
    validateBirthdate: function(birthdate){
        console.log(birthdate)
        if(!birthdate){
            this.errors.push('Favor de ingresar tu fecha de nacimiento.')
            return false;
        }else{
            const birthdateValue = new Date(birthdate)
            const currentDate = new Date();
            const eighteenYearsAgo = new Date();
            eighteenYearsAgo.setFullYear(currentDate.getFullYear() - 18)
            if (isNaN(birthdateValue.getTime())){
                this.errors.push('La fecha de nacimiento no es valida.')
                return false;
            }else if(birthdateValue > currentDate){
                this.errors.push('La fecha de nacimiento no puede ser mayor a la fecha actual.')
                return false;
            }else if(birthdateValue > eighteenYearsAgo){
                this.errors.push('Debes tener al menos 18 años.')
                return false;
            }else{
            return true;
            }
        }
    },
    validatePhoneNumber: function(phone){
        if(!phone){
            this.errors.push("El numero de telefono es obligatorio")
            return false;
        }
        if(phone && phone.length < 10) {
            this.errors.push("El número de telefono debe ser de minimo 10 caracteres")
            return false;
        }
        return true;
    },
    validateFileSize: function(file){
        const maxSize = 3 * 1024 * 1024;
        if (file && file.size > maxSize) {
            this.errors.push("El tamaño del archivo no debe superar los 3 MB.");
            return false;
        }
        return true;
    }
  },
};
</script>

<style></style>
