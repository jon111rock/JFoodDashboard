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
    <!-- sidebar  -->
    <div class="container-fluid">
      <div class="row">
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

          <!-- table -->

          <button
            type="button"
            class="btn btn-sm btn-outline-secondary mb-3 float-right"
          >
            新增商品
          </button>
          <div class="table-responsive">
            <table class="table table-striped table-sm">
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
                    <button class="btn btn-primary mr-1">編輯</button>
                    <button class="btn btn-primary">刪除</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
/* global $ */
export default {
  name: "Home",
  data() {
    return {
      products: [],
    };
  },
  mounted() {
    this.axios.get("https://localhost:5001/api/products").then((res) => {
      this.products = res.data;
    });
  },
};
</script>
<style lang="css" scoped>
@import "../assets/css/dashboard.css";
</style>
