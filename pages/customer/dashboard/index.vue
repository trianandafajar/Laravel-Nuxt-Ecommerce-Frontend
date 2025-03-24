<template>
  <div class="container-fluid mt-custom">
    <div class="fade-in">
      <div class="row">
        <div class="col-md-3">
          <!-- Sidebar -->
          <Sidebar />
        </div>

        <div class="col-md-9">
          <div class="card border-0 rounded shadow-sm border-top-orange">
            <div class="card-body">
              <h5><i class="fa fa-tachometer-alt"></i> DASHBOARD</h5>
              <hr>

              <div class="row">
                <div class="col-md-12">
                  <div class="alert alert-success" role="alert">
                    Selamat Datang, Paduka <strong>{{ $auth.user.name }}!</strong>
                  </div>
                </div>
              </div>

              <div class="row">
                <StatusCard title="PENDING" icon="fa-circle-notch fa-spin" bgColor="primary" :count="pending" />
                <StatusCard title="SUCCESS" icon="fa-check-circle" bgColor="success" :count="success" />
                <StatusCard title="EXPIRED" icon="fa-exclamation-triangle" bgColor="warning" :count="expired" />
                <StatusCard title="FAILED" icon="fa-times-circle" bgColor="danger" :count="failed" />
              </div>

            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from "@/components/web/sidebar.vue";

// Komponen kartu status untuk menghindari duplikasi
const StatusCard = {
  props: ["title", "icon", "bgColor", "count"],
  template: `
    <div class="col-6 col-lg-3">
      <div class="card rounded shadow-sm overflow-hidden">
        <div class="card-body p-0 d-flex align-items-center">
          <div :class="'bg-' + bgColor + ' py-4 px-5 mfe-3'">
            <i :class="'fas ' + icon + ' fa-2x'"></i>
          </div>
          <div>
            <div :class="'text-value text-' + bgColor">{{ count }}</div>
            <div class="text-muted text-uppercase font-weight-bold small">{{ title }}</div>
          </div>
        </div>
      </div>
    </div>
  `
};

export default {
  middleware: "isCustomer",
  layout: "default",

  components: {
    Sidebar,
    StatusCard
  },

  head() {
    return {
      title: "Dashboard - Customer",
    };
  },

  async asyncData({ $axios }) {
    try {
      const { data } = await $axios.$get("/api/customer/dashboard");

      return {
        pending: data.count?.pending || 0,
        success: data.count?.success || 0,
        expired: data.count?.expired || 0,
        failed: data.count?.failed || 0,
      };
    } catch (error) {
      console.error("Gagal mengambil data dashboard:", error);
      return { pending: 0, success: 0, expired: 0, failed: 0 };
    }
  }
};
</script>
