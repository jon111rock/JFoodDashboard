<template>
  <div>
    <!-- Title -->
    <div
      class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
    >
      <h1 class="h2">商品列表</h1>
    </div>
    <!-- Button Group -->
    <div class="btn-group float-right">
      <!-- Button -->
      <button
        type="button"
        class="btn btn-sm btn-outline-secondary mr-3"
        @click="openModal(true)"
      >
        新增商品
      </button>
      <!-- Dropdown -->
      <div class="dropdown">
        <button
          class="btn btn-secondary dropdown-toggle"
          type="button"
          id="dropdownMenuButton"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false"
        >
          {{ currentTypes.name }}
        </button>
        <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <div v-for="(item, key) in productTypes" :key="key">
            <a class="dropdown-item" @click="chooseType(item)">{{
              item.name
            }}</a>
          </div>
        </div>
      </div>
    </div>
    <!--NewProduct modal -->
    <div
      class="modal fade"
      id="addProductModal"
      tabindex="-1"
      aria-labelledby="addProductModalModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addProductModalModalLabel">
              新增商品
            </h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <div class="custom-file">
                  <input
                    type="file"
                    class="custom-file-input"
                    id="customFile"
                    lang="cn"
                    accept="image/*"
                    @change="showFile"
                  />
                  <label class="custom-file-label" for="customFile">
                    <div>{{ fileDisplay }}</div>
                  </label>
                  <button class="btn btn-primary m-3" @click="upload">
                    上傳
                  </button>
                  <div v-if="isUploadSuccess" style="color:green;">
                    上傳成功
                  </div>
                </div>
              </div>
              <div class="form-group">
                <img
                  :src="tempProduct.photoUrl"
                  style="width:150px; height:150px image-resolution: 36dpi; "
                  class="img-fluid"
                />
              </div>
              <div class="form-group">
                <label for="product-name" class="col-form-label"
                  >商品名稱 :</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="product-name"
                  v-model="tempProduct.name"
                />
              </div>
              <div class="form-group">
                <label for="product-price" class="col-form-label">價格 :</label>
                <input
                  type="number"
                  class="form-control"
                  id="product-price"
                  v-model="tempProduct.price"
                />
              </div>
              <div class="form-group">
                <label for="message-text" class="col-form-label"
                  >商品描述 :</label
                >
                <textarea
                  class="form-control"
                  id="message-text"
                  v-model="tempProduct.description"
                ></textarea>
              </div>
              <div class="form-group">
                <label for="state-bool" class="col-form-label"
                  >是否啟用 :
                </label>
                <input
                  type="checkbox"
                  id="state-bool"
                  class="d-block mx-auto"
                  v-model="tempProduct.state"
                  true-value="1"
                  false-value="0"
                />
              </div>
              <div class="form-group">
                <label for="message-text" class="col-form-label">類別 :</label>
                <p>{{ currentTypes.name }}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              取消
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="updateProduct"
            >
              確認
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- table -->
    <div class="table-responsive">
      <table class="table table-striped table-sm mt-3" v-if="isConnect">
        <thead>
          <tr>
            <th>商品圖片</th>
            <th>商品名稱</th>
            <th>價格</th>
            <th>描述</th>
            <th>啟用狀態</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in products" :key="item.id">
            <td>
              <a :href="item.photoUrl">
                <img
                  :src="item.photoUrl"
                  style="width:100px; height:100px image-resolution: 36dpi; "
                  class="img-fluid"
                />
              </a>
            </td>
            <td>{{ item.name }}</td>
            <td>{{ item.price }}</td>
            <td>{{ item.description }}</td>
            <td>
              <div v-if="item.state == 1">
                <span style="color:green;">已啟用</span>
              </div>
              <div v-else>
                <span style="color:red;">停售</span>
              </div>
            </td>
            <td>
              <button
                class="btn btn-primary mr-1"
                @click="openModal(false, item)"
              >
                編輯
              </button>
              <button class="btn btn-primary" @click="deleteProduct(item.id)">
                刪除
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <div v-else>連線失敗，請檢查伺服器狀態</div>
    </div>
  </div>
</template>

<script>
const token = "f1583d9188297b01ab127ca75ac77ab512902cb7";
const album = "Nvd1cBr";
/* global $ */
export default {
  name: "Home",
  data() {
    return {
      products: [],
      isConnect: false,
      tempProduct: {},
      tempFile: {},
      isNew: false,
      fileDisplay: "請選擇圖片",
      isUploadSuccess: false,
      //ProductType
      productTypes: [],
      currentTypes: {},
      test: [],
    };
  },
  mounted() {
    this.getProducts();
    this.getProductTypes();
  },
  methods: {
    showFile(e) {
      this.tempFile = e.target.files[0];
      this.tempProduct.photoUrl = this.tempFile.name;
      this.fileDisplay = this.tempFile.name;
    },
    upload() {
      let vm = this;

      let settings = {
        async: true,
        crossDomain: true,
        processData: false,
        contentType: false,
        type: "POST",
        dataType: "json",
        url: "https://api.imgur.com/3/image",
        headers: {
          Authorization: "Bearer " + token,
        },
        mimeType: "multipart/form-data",
      };

      let form = new FormData();
      form.append("image", this.tempFile);
      form.append("title", this.tempFile.name);
      form.append("description", "test");
      form.append("album", album); // 有要指定的相簿就加這行

      settings.data = form;

      $.ajax(settings).done(function(res) {
        vm.tempProduct.photoUrl = res.data.link;
        vm.isUploadSuccess = true;
        console.log(res.data.link); // 可以看見上傳成功後回的值
      });
    },
    getProducts() {
      this.axios
        .get("https://localhost:5001/api/products")
        .then((res) => {
          this.isConnect = true;
          this.products = res.data;
        })
        .catch((err) => {
          this.isConnect = false;
          console.log("連線失敗", err);
        });
    },
    getProductTypes() {
      this.axios
        .get("https://localhost:5001/api/producttypes")
        .then((res) => {
          res.data.forEach((i) => {
            if (i.state > 0) {
              this.productTypes.push(i);
            }
          });
          this.currentTypes = this.productTypes[0];
        })
        .catch((err) => {
          console.log("取得類別失敗", err);
        });
    },
    updateProduct() {
      var vm = this;
      if (this.isNew) {
        //新增商品
        // console.log("add");
        this.tempProduct.productTypeId = this.currentTypes.productTypeId; //自動套用類別編號
        console.log(this.tempProduct);
        this.axios
          .post("https://localhost:5001/api/products", this.tempProduct)
          .then((res) => {
            console.log("新增成功");
            vm.getProducts();
            $("#addProductModal").modal("hide");
          })
          .catch((err) => {
            console.log("失敗", err);
          });
      } else {
        //更新商品
        this.axios
          .put(
            `https://localhost:5001/api/products/${this.tempProduct.id}`,
            this.tempProduct
          )
          .then((res) => {
            console.log("成功更新商品");
            vm.getProducts();
            $("#addProductModal").modal("hide");
          })
          .catch((err) => {
            console.log(err);
          });
      }
    },

    deleteProduct(Id) {
      var vm = this;
      this.axios
        .delete(`https://localhost:5001/api/products/${Id}`)
        .then((res) => {
          console.log("刪除成功");
          vm.getProducts();
        })
        .catch((err) => {
          console.log("刪除失敗");
        });
    },
    openModal(isNew, item) {
      console.log(this.tempFile);
      if (isNew) {
        console.log("new item");
        this.isNew = isNew;
        this.tempProduct = {};
        this.tempFile = {};
        this.fileDisplay = "請選擇圖片";
        this.isUploadSuccess = false;
      } else {
        console.log("edit item");
        this.isNew = isNew;
        this.tempProduct = item;
        this.fileDisplay = item.photoUrl;
        this.isUploadSuccess = false;
      }
      $("#addProductModal").modal("show");
    },
    chooseType(item) {
      this.currentTypes = item;
    },
  },
};
</script>
