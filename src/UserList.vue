<template>
  <div class="user-list" style="background-color: #ffffff">
    <el-collapse style="margin: 4px" value="1">
      <el-collapse-item title="Participants" name="1">
        <table class="" style="padding-top: 5px">
          <tbody>
            <tr v-for="user in participants" :key="user.id">
                <td style="text-align: center;">
                  <!-- <img :src="user.imageUrl" class="img-msg"/> -->
                  <div class="sc-message--avatar align-vertical-middle card-shadow--small" :style="'background-color:hsl(' + user.imageUrl +'); display: table'">
                    <span style="text-align: center; display: table-cell; vertical-align: middle; font-weight: bold;">
                      {{ user.name.charAt(0) }}
                    </span>
                  </div>
                </td>
                <td>{{user.name}}</td>
            </tr>
          </tbody>
        </table>
      </el-collapse-item>
      <el-collapse-item title="Detail" name="2" v-if="detail">
        <table class="" style="padding-top: 5px">
          <tbody>
            <tr>
              <td>
                <b>Created Date</b>
              </td>
              <td>
                :
              </td>
              <td>
                {{ formatDate(detail.dateCreate, "DD MMM YYYY, HH:mm") }}
              </td>
            </tr>
            <tr>
              <td>
                <b>Ticket Code</b>
              </td>
              <td>
                :
              </td>
              <td>
                {{ detail.code }}
              </td>
            </tr>
            <tr>
              <td>
                <b>Problem/Issue</b>
              </td>
              <td>
                :
              </td>
              <td>
                {{ detail.body }}
              </td>
            </tr>
          </tbody>
        </table>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>
<script>
import moment from 'moment-timezone'
export default {
  props: {
    participants: {
      type: Array,
      required: true
    },
    detail: {
      type: Object,
      required: false
    }
  },
  methods: {
    formatDate(str, format){
      return moment(str).format(format)
    }
  }
}
</script>
<style scoped>
  .user-list {
    height: 100%;
    overflow: auto;
    padding-left: 5px;
    padding-top: 8px;
  }
  .img-msg {
    border-radius: 50%;
    width: 50px;
    margin-right: 5px;
  }
</style>
