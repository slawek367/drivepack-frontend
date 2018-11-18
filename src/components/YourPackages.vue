<template>
    <transition name="modal-fade">
        <div style="overflow: auto;">
            <div class="modal-backdrop">
                <div class="modal">
                    <header class="modal-header">
                        <h3>Your packages</h3>
                    </header>
                    <section class="modal-body w3-center">
                        <slot name="body">
                            <table class="w3-table-all" >
                                <tr>
                                    <th>Price</th>
                                    <th>Reciver phone</th>
                                    <th>Height</th>
                                    <th>Width</th>
                                    <th>Status</th>
                                    <th>Description</th>
                                </tr>
                                <tr v-for="item in packages" v-bind:key="item.user_id">
                                    <td>{{ item.proposed_price }}</td>
                                    <td>{{ item.receiver_phone }}</td>
                                    <td>{{ item.height }}</td>
                                    <td>{{ item.width }}</td>
                                    <td>{{ item.status }}</td>
                                    <td>{{ item.description }}</td>
                                </tr>
                            </table>
                        </slot>
                    </section>
                    <footer class="modal-footer">
                        <slot name="footer">
                        <button type="button" class="btn-green" @click="close">
                            Close
                        </button>
                        </slot>
                    </footer>
                </div>
            </div>
        </div>
    </transition>
</template>


<style scoped>
h3 {
  margin: auto;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  overflow: auto;
  z-index: 1;
}

.modal {
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  overflow: auto;
  display: flex;
  flex-direction: column;
  border: double;
  border-color: #4aae9b;
  width: 100%;
}

.modal-header,
.modal-footer {
  padding: 5px;
  display: flex;
  flex-wrap: nowrap;
}

.modal-header {
  border-bottom: 1px solid #eeeeee;
  color: #ffffff;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  justify-content: flex-end;
  display: flex;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
  display: flex;
  overflow: auto;
  width: 100%;
}

.btn-close {
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: #4aae9b;
  background: transparent;
}

.btn-green {
  color: white;
  background: #4aae9b;
  border: 1px solid #4aae9b;
  border-radius: 2px;
}

.modal-fade-enter,
.modal-fade-leave-active {
  opacity: 0;
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.5s ease;
}
</style>

<script>
import axios from "axios";

export default {
  name: "YourPackages",
  props: {
    token: {
      type: String
    }
  },
  data() {
    return {
        packages: null
    };
  },
  created: function(){
      this.getPackages()
  },
  methods: {
    close() {
      this.$emit("toggleYourPackages");
    },
    getPackages: function() {
    let config = {
      headers: {
        "x-access-token": this.token
      }
    };
    axios
      .get("http://localhost:5000/packages/my/sent/", config)
      .then(response => {
        this.packages = response.data.sent_packages
      })
      .catch(e => { console.log(e)});
    }
  }
};
</script>