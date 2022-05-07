<template>
  <section class="sortable-seaction">
    <div class="sortable-seaction__head">
      <h2 class="sortable-seaction__head__title">Sorting Training System</h2>
      <button
        @click="startModal = true"
        class="sortable-seaction__head__action"
      >
        Start sorting!
      </button>
    </div>

    <div class="container">
      <div v-if="users.length" class="sortable-seaction__info">
        <div>
          <template v-if="showTime"> Time: {{ showTime }} </template>
        </div>
        <div>{{ users.length }} peoples in the list</div>
      </div>
      <!-- data table -->
      <div class="table-responsive">
        <table class="sortable-table">
          <thead class="list__head">
            <tr>
              <th>Email</th>
              <th>Potatoes</th>
              <th>Tags</th>
              <th>Full name</th>
              <th>Location</th>
            </tr>
          </thead>
          <!-- body start -->
          <draggable
            tag="tbody"
            v-model="users"
            :animation="200"
            :ghost-class="'ghost'"
            draggable=".list__item"
            @end="checkOrder"
          >
            <tr
              v-for="user in users"
              :key="user.id"
              :class="{
                'list__item--selected':
                  user.selected || user.isChecked ? ' checked ' : '',
              }"
              class="list__item"
            >
              <td>
                <div
                  class="checkbox-div"
                  @click="user.isChecked = !user.isChecked"
                >
                  <input
                    :id="`isChecked-${user.id}`"
                    type="checkbox"
                    v-model="user.isChecked"
                    class="checkbox"
                  />
                  <label
                    :htmlFor="`isChecked-${user.id}`"
                    class="isCheckedLabel d-flex flex-items-center"
                  >
                    <span />
                    {{ user.email }}
                  </label>

                  <span class="right-arrow" />
                </div>
              </td>
              <td>{{ user.potatoes }}</td>
              <td>
                <ul class="tag">
                  <li class="tag__item">{{ user.tags }}</li>
                  <li class="tag__item">VIP</li>
                  <li class="tag__item tag__item--more">+1</li>
                </ul>
              </td>
              <td>{{ user.fullName }}</td>
              <td>{{ user.location }}</td>
            </tr>
          </draggable>
          <!-- body end -->
        </table>
        <p v-if="users.length === 0" class="text-center">
          Data not loaded yet!
        </p>
        <!-- table -->
      </div>
    </div>

    <!--  generate people modal start -->
    <Modal
      v-if="startModal"
      @start="generatePeopleList"
      @close="startModal = false"
      :value="selectedRange"
      v-model.number="selectedRange"
    />
    <!--  generate people modal  end -->
    <SuccessModal
      :message="message"
      :move="move"
      @restart="restart"
      @close="successModal = false"
      v-if="successModal"
    />
  </section>
</template>

<script>
import { faker } from "@faker-js/faker";
import draggable from "vuedraggable";
import Modal from "../components/Modal.vue";
import SuccessModal from "../components/SuccessModal.vue";
export default {
  name: "SortableHome",
  components: {
    draggable,
    Modal,
    SuccessModal,
  },
  data() {
    return {
      startModal: false,
      successModal: false,
      time: null,
      timer: null,
      showTime: null,
      move: 0,
      order: [],
      users: [],
      selectedRange: 20,
      message: "",
    };
  },
  methods: {
    /**
     * Get Generate People List
     */
    async generatePeopleList() {
      let people = [];
      let order = new Set();

      for (let i = 0; i < this.selectedRange; i++) {
        let potatoes = faker.datatype.number({ min: 1, max: 1000 });
        order.add(potatoes);

        people.push({
          id: faker.datatype.uuid(),
          isChecked: false,
          email: faker.internet.email(),
          potatoes: potatoes,
          tags: faker.lorem.word(),
          fullName: faker.name.findName(),
          location: faker.address.country(),
        });
      }

      this.users = people;
      this.order = [...order].sort((a, b) => b - a);
      this.time = Date.now();
      this.timer = setInterval(this.setTimer, 1000);
      this.startModal = false;
    },

    /**
     * Start pad with 0
     *
     * @param {String|Number} value
     *
     * @return {String}
     */
    startPad(value) {
      return value.toString().padStart(2, "0");
    },

    /**
     * Set timer
     */
    setTimer() {
      let TIME_LENGTHS = [3600, 60, 1];
      let messageJoins = [];
      let diffInSeconds = parseInt((Date.now() - this.time) / 1000);

      for (let tl of TIME_LENGTHS) {
        let num_of_times = 0;

        if (diffInSeconds >= tl) {
          num_of_times = (diffInSeconds / tl).toFixed(0);
          diffInSeconds = diffInSeconds % tl;
        }

        messageJoins.push(this.startPad(num_of_times));
      }

      this.showTime = messageJoins.join(":");
    },

    /**
     * Get readable date from now by given past date
     *
     * @param {Object} start
     *
     * @return {String}
     */
    getReadableTimeFromNow(start) {
      let TIME_LENGTHS = [
        { seconds: 31536000, term: "years" },
        { seconds: 2592000, term: "months" },
        { seconds: 86400, termterm: "days" },
        { seconds: 3600, term: "hours" },
        { seconds: 60, term: "minutes" },
        { seconds: 1, term: "seconds" },
      ];
      let messageJoins = [];
      let diffInSeconds = parseInt((Date.now() - start.getTime()) / 1000);

      for (let tl of TIME_LENGTHS) {
        if (diffInSeconds >= tl.seconds) {
          let num_of_times = (diffInSeconds / tl.seconds).toFixed(0);

          diffInSeconds = diffInSeconds % tl.seconds;

          let term =
            num_of_times > 1
              ? tl.term
              : tl.term.substring(0, tl.term.length - 1);

          messageJoins.push(`${num_of_times} ${term}`);
        }
      }

      return messageJoins.join(", ");
    },

    /**
     * Check wheather sorting done or not
     */
    checkOrder() {
      let ordered = this.users.every(
        (user, index) => this.order[index] === user.potatoes
      );

      this.move++;
   console.log(ordered);
      if (!ordered) {
        return;
      }

      clearInterval(this.timer);

      this.message = this.getReadableTimeFromNow(new Date(this.time));
      this.successModal = true;
      this.escapedTime = "";
    },

    /**
     * Restart sorting
     */
    restart() {
      this.reset();
      
      this.successModal = false;
      this.startModal = true;
    },

    /**
     * Reset necessary data to default value
     */
    reset() {
      clearInterval(this.timer);

      this.time = null;
      this.timer = null;
      this.escapedTime = null;
      this.move = 0;
      this.order = [];
      this.users = [];
      this.selectedRange = 20;
      this.message = "";
    },
  },
};
</script>
<style lang="sass" scoped>
@import '../styles/_table.scss';
</style>
