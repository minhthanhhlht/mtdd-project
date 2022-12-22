><template>
  <div class="pricing-header px-3 py-3 pt-md-5 pb-md-4 mx-auto text-center">
    <h3 class="display-5">Product Infomation</h3>
    <router-link to="/product">Back</router-link>
  </div>

  <div class="container">
    <form @submit.prevent="save()">
      <div class="form-group row">
        <label for="inputPassword" class="col-sm-3 col-form-label"
          >Product name</label
        >
        <div class="col-sm-9">
          <input
            type="text"
            class="form-control"
            v-model="products.name"
            v-bind:class="{ 'is-invalid': errors.name }"
          />
          <div class="invalid-feedback">{{ errors.name }}</div>
        </div>
      </div>
      <div class="form-group row">
        <label for="inputPassword" class="col-sm-3 col-form-label"
          >Product price</label
        >
        <div class="col-sm-9">
          <input
            type="text"
            class="form-control"
            v-model="products.price"
            v-bind:class="{ 'is-invalid': errors.price }"
          />
          <div class="invalid-feedback">{{ errors.price }}</div>
        </div>
      </div>
      <div class="form-group row">
        <label for="inputPassword" class="col-sm-3 col-form-label"
          >Product description</label
        >
        <div class="col-sm-9">
          <textarea
            class="form-control"
            rows="3"
            v-model="products.description"
            v-bind:class="{ 'is-invalid': errors.description }"
          ></textarea>
          <div class="invalid-feedback">{{ errors.description }}</div>
        </div>
      </div>
      <div class="form-group row">
        <label for="inputPassword" class="col-sm-3 col-form-label"></label>
        <div class="col-sm-9 text-left">
          <button type="submit" class="btn btn-primary">Save</button> &nbsp;
          <button type="reset" class="btn btn-danger">Cancel</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "ProductForm",
  data() {
    return {
      errors: {
        name: "",
        price: "",
        description: "",
      },
      products: {
        name: "",
        price: "",
        description: "",
      },
    };
  },
  created() {
    let productId = this.$route.params.id
    if (productId) {
      this.getProduct(productId)
    }
  },
  methods: {
    validate() {
      let isValid = true;
      this.errors = {
        name: "",
        price: "",
        description: "",
      };
      if (!this.products.name) {
        this.errors.name = "Please enter your name product";
        isValid = false;
      }
      if (!this.products.price) {
        this.errors.price = "Please enter your price product";
        isValid = false;
      } else if (!this.isNumber(this.products.price)) {
        this.errors.price = "Please enter number";
        isValid = false;
      }
      if (!this.products.description) {
        this.errors.description = "Please enter your description product";
        isValid = false;
      }
      return isValid;
    },
    isNumber(value) {
      return /^\d*$/.test(value);
    },
    save() {
      // this.validate();
      if (this.validate()) {
        this.$request
          .post("http://localhost:9000/api/products/", this.products)
          .then((res) => {
            if (res.data.success) {
              this.$router.push({ name: "product.list" });
              return;
            }
            alert("Error");
          });
      }
    },
    getProduct(productId) {
     
      this.$request.get(`http://localhost:9000/api/products/${productId}`).then((res) => {        
           this.products = res.data[0];
           
        });
    }
  },
};
</script>