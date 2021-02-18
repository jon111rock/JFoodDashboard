<template>
  <div>
    <!-- Title -->
    <div
      class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
    >
      <h1 class="h2">商品類別</h1>
    </div>
    <!-- Button -->
    <button
      type="button"
      class="btn btn-sm btn-outline-secondary mb-3 float-right"
      @click="openProductTypeModal"
    >
      新增類別
    </button>
    <!-- NewProductType Modal -->
    <div
      class="modal fade"
      id="productTypeModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">
              新增類別
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
                <label for="recipient-name" class="col-form-label"
                  >類別名稱:</label
                >
                <input
                  type="text"
                  class="form-control"
                  id="recipient-name"
                  v-model="tempProductType.name"
                />
              </div>
              <div class="form-group">
                <label for="state-bool" class="col-form-label"
                  >是否啟用 :
                </label>
                <input
                  type="checkbox"
                  id="state-bool"
                  class="d-block mx-auto"
                  v-model="tempProductType.state"
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
              @click="updateProductType"
            >
              確定
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- table -->
    <div class="table-responsive">
      <table class="table table-striped table-sm mt-3">
        <thead>
          <tr>
            <th>名稱</th>
            <th>啟用狀態</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>test</td>
            <td>已啟用</td>
            <td>
              <button class="btn btn-primary">test</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      productTypes: {},
      tempProductType: {},
    };
  },
  mounted() {
    this.getProductType();
  },
  methods: {
    //ProductType
    getProductType() {
      console.log("取得列表");
      var vm = this;
      this.axios
        .get("https://localhost:5001/api/producttypes")
        .then((res) => {
          vm.productTypes = res.data;
        })
        .catch((err) => {
          console.log("無法取得類別清單", err);
        });
    },
    updateProductType() {
      var vm = this;
      this.axios
        .post("https://localhost:5001/api/producttypes", this.tempProductType)
        .then((res) => {
          console.log("新增類別成功");
          $("#producgtTypeModal").modal("hide");
          vm.getProductType();
        })
        .catch((err) => {
          console.log("新增類別失敗", err);
        });
    },
    openProductTypeModal() {
      this.tempProductType = {};
      $("#productTypeModal").modal("show");
    },
  },
};
</script>
