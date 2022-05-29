<template>
  <div>
    <h1>main app</h1>
    <ProductAdmin
      :products="products"
      @add-product-to-list="addProductToList"
    />
  </div>
</template>

<script>
import ProductAdmin from "@/components/admin/ProductAdmin";
export default {
  components: {
    ProductAdmin,
  },
  data() {
    return {
      products: [
        {
          id: 1,
          title: "Churu clásico unidad",
          price: 1500,
          desc: "Clásico churu sabor atún.",
          img: "churu.jpg",
          highlight: false,
          stock: 2,
        },
        {
          id: 2,
          title: "Leonardo atún",
          price: 3500,
          desc: "Exclusivo sabor atún.",
          img: "leonardo.jpg",
          highlight: false,
          stock: 0,
        },
        {
          id: 3,
          title: "Leonardo salmón",
          price: 1500,
          desc: "Sabor salmón ahumado.",
          img: "leonardo_2.jpg",
          highlight: false,
          stock: 2,
        },
        // {
        //   id: 4,
        //   title: "Mattise exclusive",
        //   price: 4500,
        //   desc: "Alto estandar de calidad.",
        //   img: "mattise.jpg",
        //   highlight: false,
        //   stock: 5,
        // },
        // {
        //   id: 5,
        //   title: "Proplan Urinary",
        //   price: 1500,
        //   desc: "Para el cuidado renal del michi",
        //   img: "proplan_urinary.jpg",
        //   highlight: false,
        //   stock: 2,
        // },
        // {
        //   id: 6,
        //   title: "Bravery",
        //   price: 4800,
        //   desc: "La mejor de todas",
        //   img: "bravery.jpg",
        //   highlight: false,
        //   stock: 0,
        // },
      ],
    };
  },
  methods: {
    addProductToList(product) {
      this.products.push(JSON.parse(product))  
    },
  },
};
</script>

<style>
</style>