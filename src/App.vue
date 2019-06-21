<template>
  <mdb-container>
    <mdb-row>
      <mdb-col col="12">
        <mdb-navbar dark color="indigo">
          <!-- Navbar brand -->
          <mdb-navbar-brand href="#">Byte 360</mdb-navbar-brand>
          <mdb-navbar-toggler>
            <mdb-navbar-nav right>
              <mdb-nav-item href="#" active>Home</mdb-nav-item>
              <mdb-nav-item href="#">About</mdb-nav-item>
              <mdb-nav-item class="disabled" href="#">Contact</mdb-nav-item>
              <mdb-dropdown tag="li" class="nav-item">
                <mdb-dropdown-toggle
                  tag="a"
                  navLink
                  color="indigo"
                  slot="toggle"
                  waves-fixed
                >Packages</mdb-dropdown-toggle>
                <mdb-dropdown-menu>
                  <mdb-dropdown-item>Action</mdb-dropdown-item>
                  <mdb-dropdown-item>Another action</mdb-dropdown-item>
                  <mdb-dropdown-item>Something else here</mdb-dropdown-item>
                </mdb-dropdown-menu>
              </mdb-dropdown>
            </mdb-navbar-nav>
          </mdb-navbar-toggler>
        </mdb-navbar>
      </mdb-col>
    </mdb-row>
    <mdb-row>
      <mdb-col col="9">
        <h2 class="text-uppercase my-3">Today:</h2>
        <Event
          v-for="(event, index) in events"
          :index="index"
          :time="event.time"
          :title="event.title"
          :location="event.location"
          :description="event.description"
          :key="index"
          @delete="handleDelete"
        />
        <mdb-row>
          <mdb-col xl="3" md="6" class="mx-auto text-center">
            <mdb-btn color="info" @click="showModel(true)">Add Event</mdb-btn>
          </mdb-col>
        </mdb-row>
      </mdb-col>
      <mdb-col col="3">
        <h3 class="text-uppercase my-3">Schedule</h3>
        <h6 class="my-3">
          It's going to be busy that today. You have
          <b>{{events.length}} events</b> today.
        </h6>
        <h1 class="my-3">
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon far icon="sun"/>
            </mdb-col>
            <mdb-col col="9">Sunny</mdb-col>
          </mdb-row>
          <mdb-row>
            <mdb-col col="3" class="text-center">
              <mdb-icon icon="thermometer-three-quarters"/>
            </mdb-col>
            <mdb-col col="9">23°C</mdb-col>
          </mdb-row>
        </h1>
        <p>
          Don't forget your sunglasses. Today will dry and sunny, becoming
          warm in the afternoon with temperatures of between 20 and 25
          degrees.
        </p>
      </mdb-col>
    </mdb-row>
    <mdb-modal v-if="modal">
      <mdb-modal-header>
        <mdb-modal-title tag="h4" class="w-100 text-center font-weight-bold">Add Event!</mdb-modal-title>
      </mdb-modal-header>
      <mdb-modal-body>
        <form class="mx-3 grey-text" novalidate>
          <div class="field" :class="{error: errors.has('time')}">
            <mdb-input
              name="time"
              label="Time"
              icon="clock"
              placeholder="12:30"
              type="text"
              @input="handleInput($event, 'time')"
              v-validate="'required'"
              v-model="time"
            />
            <div class="error" v-if="errors.has('time')">{{errors.first('time')}}</div>
          </div>
          <div class="field" :class="{error: errors.has('title')}">
            <mdb-input
              name="title"
              label="Title"
              icon="edit"
              placeholder="Briefing"
              type="text"
              @input="handleInput($event, 'title')"
              v-validate="'required'"
              v-model="title"
            />
            <div class="error" v-if="errors.has('title')">{{errors.first('title')}}</div>
          </div>
          <div class="field" :class="{error: errors.has('location')}">
            <mdb-input
              name="location"
              label="Location (optional)"
              icon="map"
              type="text"
              @input="handleInput($event, 'location')"
              v-validate="'required'"
              v-model="location"
            />
            <div class="error" v-if="errors.has('location')">{{errors.first('location')}}</div>
          </div>
          <mdb-textarea
            name="description"
            label="Description (optional)"
            icon="sticky-note"
            @input="handleInput($event, 'description1')"
          />
        </form>
      </mdb-modal-body>
      <mdb-modal-footer class="justify-content-center">
        <mdb-btn type="submit" color="info" @click="onSubmit">Add Event</mdb-btn>
        <mdb-btn color="info" @click="showModel(false)">Cancel</mdb-btn>
      </mdb-modal-footer>
    </mdb-modal>
  </mdb-container>
</template>

<script>
import Vue from "vue";
import VeeValidate from "vee-validate";
Vue.use(VeeValidate);
import Hello from "@/components/hello";
//import Grid from '@/components/grid';
import Event from "@/components/Event";
import {
  mdbContainer,
  mdbRow,
  mdbCol,
  mdbBtn,
  mdbIcon,
  mdbModal,
  mdbModalHeader,
  mdbModalTitle,
  mdbModalBody,
  mdbModalFooter,
  mdbInput,
  mdbTextarea,
  mdbNavbar,
  mdbNavbarBrand,
  mdbNavbarToggler,
  mdbNavbarNav,
  mdbNavItem,
  mdbDropdown,
  mdbDropdownMenu,
  mdbDropdownToggle,
  mdbDropdownItem,
  mdbFormInline
} from "mdbvue";
export default {
  name: "App",
  components: {
    Hello,
    //Grid,
    mdbContainer,
    mdbRow,
    mdbCol,
    Event,
    mdbBtn,
    mdbIcon,
    mdbModal,
    mdbModalHeader,
    mdbModalTitle,
    mdbModalBody,
    mdbModalFooter,
    mdbInput,
    mdbTextarea,
    Event,
    mdbNavbar,
    mdbNavbarBrand,
    mdbNavbarToggler,
    mdbNavbarNav,
    mdbNavItem,
    mdbDropdown,
    mdbDropdownMenu,
    mdbDropdownToggle,
    mdbDropdownItem,
    mdbFormInline
  },
  data() {
    return {
      newValues: {},
      time: "",
      events: [
        {
          time: "10:00 PM",
          title: "Anuual Meeting",
          location: "Hamdard University",
          description: "This is our annual party"
        },
        {
          time: "10:00 PM",
          title: "Anuual Meeting",
          location: "Hamdard University",
          description: "This is our annual party"
        },
        {
          time: "10:00 PM",
          title: "Anuual Meeting",
          location: "Hamdard University",
          description: "This is our annual party"
        },
        {
          time: "10:00 PM",
          title: "Anuual Meeting",
          location: "Hamdard University",
          description: "This is our annual party"
        }
      ],

      modal: false
    };
  },

  methods: {
    onSubmit() {
      this.$validator.validateAll().then(result => {
        if (result) {
          this.events.push({
            time: this.newValues["time"],
            title: this.newValues["title"],
            location: this.newValues["location"],
            description: this.newValues["description"]
          });
          this.newValues = {};
          this.showModel(false);
        }
      });
    },
    handleDelete(eventIndex) {
      this.events.splice(eventIndex, 1);
    },
    handleInput(val, type) {
      this.newValues[type] = val;
    },
    showModel(event) {
      this.modal = event;
    }
  }
};
</script>

<style>
div.error {
  color: #9f3a38;
}
</style>
