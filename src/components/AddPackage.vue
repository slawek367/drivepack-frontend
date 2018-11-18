<template>
    <transition name="modal-fade">
        <div style="overflow: auto;">
            <div class="modal-backdrop">
                <div class="modal">
                    <header class="modal-header">
                        <h3>Add Package</h3>
                    </header>
                    <section class="modal-body w3-center">
                        <slot name="body">
                            <form class="w3-container w3-center" style="width: 100%; height: 100%; overflow-x: scroll;" @submit.prevent="addPackage">
                                <div class="w3-section">
                                        <input required v-model="receiver_phone" type="number" placeholder="Receiver phone" class="w3-input w3-border w3-margin-bottom"/>
                                        <!--<input required v-model="from_x_pos" type="number" placeholder="from_x_pos" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="from_y_pos" type="number" placeholder="from_y_pos" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="to_x_pos" type="number" placeholder="to_x_pos" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="to_y_pos" type="number" placeholder="to_y_pos" class="w3-input w3-border w3-margin-bottom"/> -->
                                        <input required v-model="date_from" type="date" placeholder="Date from" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="date_to" type="date" placeholder="Date to" class="w3-input w3-border w3-margin-bottom"/>
                                        <!-- <input required v-model="max_deliver_time_minutes" type="number" placeholder="Max deliver time (minutes)" class="w3-input w3-border w3-margin-bottom"/> -->
                                        <input required v-model="proposed_price" type="number" placeholder="Proposed price (PLN)" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="weight" type="number" placeholder="Weight (g)" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="height" type="number" placeholder="Height (cm)" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="width" type="number" placeholder="Width (cm)" class="w3-input w3-border w3-margin-bottom"/>
                                        <input required v-model="description" type="text" placeholder="Description" class="w3-input w3-border w3-margin-bottom"/>
                                    <button type="submit" class="w3-button w3-block w3-green w3-section w3-padding">Add</button>
                                    <div v-if="packageAdded" class="w3-panel w3-pale-green w3-border">
                                        <h4>Package added!</h4>
                                    </div>
                                </div>
                            </form>

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
  name: "AddPackage",
  props: {
    token: {
      type: String
    }
  },
  data() {
    return {
      receiver_phone: "",
      from_x_pos: 50.0281837,
      from_y_pos: 19.9087314,
      to_x_pos: 50.0281937,
      to_y_pos: 19.9087414,
      date_from: "",
      date_to: "",
      max_deliver_time_minutes: 1440,
      proposed_price: "",
      weight: "",
      height: "",
      width: "",
      description: "",
      packageAdded: false
    };
  },
  methods: {
    close() {
      this.$emit("toggleAddPackage");
    },
    addPackage: function() {
      axios
        .post("http://localhost:5000/packages", {
          receiver_phone: this.receiver_phone,
          from_x_pos: this.from_x_pos,
          from_y_pos: this.from_y_pos,
          to_x_pos: this.to_x_pos,
          to_y_pos: this.to_y_pos,
          date_from: this.date_from,
          date_to: this.date_to,
          max_deliver_time_minutes: this.max_deliver_time_minutes,
          proposed_price: this.proposed_price,
          weight: this.weight,
          height: this.height,
          width: this.width,
          description: this.description,
          token: this.token // TODO, this is some workaround to provide token
        })
        .then(response => {
          console.log("ok");
          this.packageAdded = true

          setTimeout(() => {
              this.packageAdded = false
              this.receiver_phone= "",
              this.date_from= "",
              this.date_to= "",
              this.proposed_price= "",
              this.weight= "",
              this.height= "",
              this.width= "",
              this.description= ""
          }, 2000)
        })
        .catch(e => {
          console.log(e);
        });
    }
  }
};
</script>