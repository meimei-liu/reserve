<template>
  <v-container class="reserve" fluid width="1000px">
    <v-row justify="center" >
      <v-col cols="12" sm="8" md="6" >
        <v-card elevation="2" class="frosted-glass">
          <v-card-title class="headline">预定信息查询系统</v-card-title>
          <v-card-text >
            <v-form @submit.prevent="searchReservations">
              <v-text-field
                v-model="userId"
                label="请输入用户ID"
                prepend-icon="mdi-account"
                clearable
                variant="solo"
                class="custom-icon-size"
              ></v-text-field>
              <v-btn
                prepend-icon="mdi-check-circle"
                type="submit"
                :loading="isLoading"
                :disabled="isLoading"
                color="#FF5555"
                @click="searchReservations"
                width="100px"
              >
                <template v-if="isLoading">
                  <v-progress-circular indeterminate size="12" color="red"></v-progress-circular>
                </template>
                <template v-else>
                  查询预定
                </template>
              </v-btn>
            </v-form>
          </v-card-text>
          <v-card-subtitle v-if="isLoading" class="text-center">正在加载数据...</v-card-subtitle>
          <v-alert v-if="error" type="error" dismissible>{{ error }}</v-alert>
          <v-card v-if="bookingInfo" class="mt-3 trans">
            <v-card-title>预定信息</v-card-title>
            <v-card-text >
              <v-list class="trans">
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>开始日期：</v-list-item-title>
                    <v-list-item-subtitle>{{ bookingInfo.startDate }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>结束日期：</v-list-item-title>
                    <v-list-item-subtitle>{{ bookingInfo.endDate }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>房间型号：</v-list-item-title>
                    <v-list-item-subtitle>{{ bookingInfo.roomType }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card-text>
          </v-card>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import backgroundImage from '@/assets/background-image1.png';
import axios from 'axios';

export default {
  data() {
    return {
      userId: '',
      isLoading: false,
      error: null,
      bookingInfo: null,
    };
  },
  methods: {
    async searchReservations() {
      this.isLoading = true;
      this.error = null;
      this.bookingInfo = null;

      try {
        const response = await axios.get(`https://duke486.com/api/hotel/${this.userId}`);
        if (response.data.data) {
          this.bookingInfo = response.data.data;
        } else {
          this.error = '未找到预定信息';
        }
      } catch (err) {
        this.error = err.response?.data?.message || '查询失败，请稍后重试';
      } finally {
        this.isLoading = false;
      }
    },
  },
};
</script>

<style>
.trans {
  background: rgba(255, 255, 255, 0.0)!important;
  font-color: rgba(253, 5, 5, 0)!important;
  /* backdrop-filter: blur(10px); */
  
  
}

.reserve {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 90vh;
}

body {
  background-image: url('@/assets/background-image1.png');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
}

.frosted-glass {
  background: rgba(255, 255, 255, 0.7) !important;
  backdrop-filter: blur(8px) !important;
}

.v-card {
  border: none !important;
  box-shadow: none !important;
  margin-bottom: 0px !important;
  border-radius: 13px!important;
}

.v-btn {
  border-radius: 10px!important;
  width: 100px !important;
}


.v-card-text {
 color: rgb(57, 47, 47)!important;
}

.headline {
  color: #14630d !important;
}

.v-list-item-title, .v-list-item-subtitle {
  color: #14630d!important;
}

</style>