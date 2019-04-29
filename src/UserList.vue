<template>
  <div class="user-list">
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
  </div>
</template>
<script>
export default {
  props: {
    participants: {
      type: Array,
      required: true
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
