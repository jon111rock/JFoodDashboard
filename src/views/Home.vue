<template>
  <div class="home">
    <!-- navbar -->
    <nav
      class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow"
    >
      <a class="navbar-brand col-md-3 col-lg-2 mr-0 px-3" href="#">甲J好食</a>
      <button
        class="navbar-toggler position-absolute d-md-none collapsed"
        type="button"
        data-toggle="collapse"
        data-target="#sidebarMenu"
        aria-controls="sidebarMenu"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
          <a class="nav-link" href="#">Sign out</a>
        </li>
      </ul>
    </nav>
    <div class="container-fluid">
      <div class="row">
        <!-- sidebar  -->
        <nav
          id="sidebarMenu"
          class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
        >
          <div class="sidebar-sticky pt-3">
            <ul class="nav flex-column">
              <li class="nav-item">
                <a class="nav-link" href="#">
                  <span data-feather="shopping-cart"></span>
                  Products
                </a>
              </li>
            </ul>
          </div>
        </nav>

        <main role="main" class="col-md-9 ml-sm-auto col-lg-10 px-md-4">
          <div
            class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
          >
            <h1 class="h2">Dashboard</h1>
            <div class="btn-toolbar mb-2 mb-md-0">
              <div class="btn-group mr-2">
                <button type="button" class="btn btn-sm btn-outline-secondary">
                  Share
                </button>
                <button type="button" class="btn btn-sm btn-outline-secondary">
                  Export
                </button>
              </div>
              <div class="dropdown">
                <button
                  class="btn btn-secondary dropdown-toggle"
                  type="button"
                  id="dropdownMenuButton"
                  data-toggle="dropdown"
                  aria-haspopup="true"
                  aria-expanded="false"
                >
                  Dropdown button
                </button>
                <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
                  <a class="dropdown-item" href="#">Action</a>
                  <a class="dropdown-item" href="#">Another action</a>
                  <a class="dropdown-item" href="#">Something else here</a>
                </div>
              </div>
            </div>
          </div>
          <button
            type="button"
            class="btn btn-sm btn-outline-secondary mb-3 float-right"
            @click="openModal(true)"
          >
            新增商品
          </button>

          <!-- modal -->
          <div
            class="modal fade"
            id="exampleModal"
            tabindex="-1"
            aria-labelledby="exampleModalLabel"
            aria-hidden="true"
          >
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="exampleModalLabel">
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
                      <label for="product-price" class="col-form-label"
                        >價格 :</label
                      >
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
                    新增
                  </button>
                </div>
              </div>
            </div>
          </div>
          <!-- table -->
          <div class="table-responsive">
            <table class="table table-striped table-sm" v-if="isConnect">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>商品名稱</th>
                  <th>價格</th>
                  <th>描述</th>
                  <th>啟用狀態</th>
                  <th>操作</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in products" :key="item.id">
                  <td>{{ item.id }}</td>
                  <td>{{ item.name }}</td>
                  <td>{{ item.price }}</td>
                  <td>{{ item.description }}</td>
                  <td>{{ item.state }}</td>
                  <td>
                    <button
                      class="btn btn-primary mr-1"
                      @click="openModal(false, item)"
                    >
                      編輯
                    </button>
                    <button
                      class="btn btn-primary"
                      @click="deleteProduct(item.id)"
                    >
                      刪除
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
            <div v-else>連線失敗，請檢查伺服器狀態</div>
          </div>
        </main>
      </div>
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
    };
  },
  mounted() {
    this.getProducts();
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
          console.log("連線失敗");
        });
    },
    updateProduct() {
      var vm = this;
      if (this.isNew) {
        //新增商品
        console.log("add");
        console.log(this.tempProduct);
        this.axios
          .post("https://localhost:5001/api/products", this.tempProduct)
          .then((res) => {
            console.log("新增成功");
            vm.getProducts();
            $("#exampleModal").modal("hide");
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
            $("#exampleModal").modal("hide");
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
      $("#exampleModal").modal("show");
    },
  },
};
</script>
<style lang="css" scoped>
@import "../assets/css/dashboard.css";
</style>
