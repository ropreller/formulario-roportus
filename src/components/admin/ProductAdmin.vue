<template>
  <div>
    <b-form v-if="show">
      <b-form-group
        id="input-group-1"
        label="ID del Producto"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          v-model="form.id"
          placeholder="Ingrese ID del producto."
          type="number"
          required
          @keyup="validateId"
        ></b-form-input>
        <span v-if="alert.id" class="alert-danger">
          {{ alert.id.text }}
        </span>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="Nombre del producto"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="form.title"
          placeholder="Ingrese el nombre del producto."
          required
          @keyup="validateTitle"
        ></b-form-input>
        <span v-if="alert.title" class="alert-danger">
          {{ alert.title.text }}
        </span>
      </b-form-group>

      <b-form-group id="input-group-3" label="Descripción" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.desc"
          placeholder="Ingrese la descripción del producto"
          required
          @keyup="validateDesc"
        ></b-form-input>
        <span v-if="alert.desc" class="alert-danger">
          {{ alert.desc.text }}
        </span>
      </b-form-group>

      <b-form-group id="input-group-4" label="Precio" label-for="input-4">
        <b-form-input
          id="input-4"
          v-model="form.price"
          placeholder="Ingrese el precio"
          type="number"
          required
          @keyup="validatePrice"
        ></b-form-input>
        <span v-if="alert.price" class="alert-danger">
          {{ alert.price.text }}
        </span>
      </b-form-group>

      <b-form-group id="input-group-5" label="Imagen" label-for="input-5">
        <b-form-input
          id="input-5"
          v-model="form.img"
          placeholder="Ingrese la URL de la imagen"
          required
          @keyup="validateImg"
        ></b-form-input>
        <span v-if="alert.img" class="alert-danger">
          {{ alert.img.text }}
        </span>
      </b-form-group>

      <b-form-group id="input-group-7" label="Stock" label-for="input-7">
        <b-form-input
          id="input-7"
          v-model="form.stock"
          placeholder="Ingrese el stock del producto"
          type="number"
          required
          @keyup="validateStock"
        ></b-form-input>
        <span v-if="alert.stock" class="alert-danger">
          {{ alert.stock.text }}
        </span>
      </b-form-group>

      <b-button v-if="form.showBtn" @click="addProduct" variant="primary"
        >Añadir producto</b-button
      >
      <b-button v-else variant="secondary">Faltan campos</b-button>
      <b-button @click="resetForm" type="reset" variant="danger"
        >Reset</b-button
      >
    </b-form>
    <b-table striped hover :items="items"></b-table>
  </div>
</template>

<script>
export default {
  props: {
    products: {
      type: Array,
    },
  },
  data() {
    return {
      form: {
        id: "",
        title: "",
        desc: "",
        price: "",
        img: "",
        highlight: true,
        stock: "",
        showBtn: false,
      },
      show: true,
      items: this.products,
      alert: {
        id: {
          text: "",
          validated: false,
        },
        title: {
          text: "",
          validated: false,
        },
        desc: {
          text: "",
          validated: false,
        },
        price: {
          text: "",
          validated: false,
        },
        img: {
          text: "",
          validated: false,
        },
        stock: {
          text: "",
          validated: false,
        },
      },
    };
  },
  methods: {
    addProduct() {
      this.form.highlight = true;
      this.$emit("add-product-to-list", JSON.stringify(this.form));
      this.resetForm();
    },
    resetForm() {
      // Reset our form values
      this.form.id = "";
      this.form.title = "";
      this.form.desc = "";
      this.form.img = "";
      this.form.price = "";
      //this.form.highlight = [];
      this.form.stock = 0;
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    validateId() {
      // Valido que al menos el valor del ID sea mayor a 0
      this.alert.id.text =
        this.form.id < 1 ? "ID debe contener valor numérico válido." : ""
      this.alert.id.validated = this.form.id < 1 ? false : true
      this.showAddBtn
    },
    validateTitle() {
      this.alert.title.text =
        this.form.title.length < 3
          ? "Nombre producto debe contener al menos 3 caractéres"
          : "";
      this.alert.title.validated = this.form.title.length < 3 ? false : true;
    },
    validateDesc() {
      // Para el nombre completo (mínimo 2 palabras, mínimo 2 caracteres cada una):
      const fullNameRegExp = /[a-zA-Z]{2,}\s+[a-zA-Z]{2,12}/g;
      if (fullNameRegExp.test(this.form.desc)) {
        this.alert.desc.text = ""
        this.alert.desc.validated = true
      } else {
        this.alert.desc.text =
          "Debe al menos contener 2 palabras y 2 careactéres C/U."
        this.alert.desc.validated = false
      }
      this.showAddBtn
    },
    validatePrice() {
      // Valido que al menos el valor del ID sea mayor a 0
      this.alert.price.text =
        this.form.price < 1
          ? "Precio debe contener valor numérico válido y mayor a cero."
          : "";
      this.alert.price.validated = this.form.price < 1 ? false : true
      this.showAddBtn
    },
    validateImg() {
      this.alert.img.text =
        this.form.img.length < 3
          ? "Imagen producto debe contener al menos 3 caractéres"
          : "";
      this.alert.img.validated = this.form.img.length < 3 ? false : true
      this.showAddBtn
    },
    validateStock() {
      // Valido que al menos el valor del ID sea mayor a 0
      this.alert.stock.text =
      this.form.stock.length < 1 ? "Debe insertar mínimo un valor." : ""
      this.alert.stock.validated = this.form.stock.length < 1 ? false : true
      this.showAddBtn
    },
  },
  computed: {
    showAddBtn() {
      if (
        this.alert.id.validated &&
        this.alert.title.validated &&
        this.alert.desc.validated &&
        this.alert.price.validated &&
        this.alert.img.validated &&
        this.alert.stock.validated
      ) {
        return (this.form.showBtn = true);
      } else {
        return (this.form.showBtn = false);
      }
    },
  },
};
</script>
