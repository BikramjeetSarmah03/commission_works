<!-- As it's using tailwind, u can install the tailwind Css Intellisense from the extensions, and just hover over the classes to see the css, btw some classes are used for custom css also so there it will not show their css -->

<template>
  <div id="app">
    <!-- Side Man Image -->
    <div class="fixed bottom-0 z-50">
      <img
        src="@/assets/images/hman.png"
        alt="hman"
        class="fixed bottom-0 -scale-x-100 translate-y-[400px] left-[-100px] hidden md:block hman_comeIn"
      />
      <!-- By default it's hidden -->
      <div
        class="msg_box_outer -rotate-4 msg_box_slideIn"
        style="display: none"
      >
        <div class="-mt-5 rotate-4 msg_box_inner">
          <div class="message">Message Box</div>
        </div>
      </div>
    </div>

    <!-- Navbar -->
    <div class="header">
      <button class="hamburgerIcon" @click.prevent="toggle()">
        <div :class="[!isSidebar ? '' : 'active']"></div>
        <div :class="[!isSidebar ? '' : 'active']"></div>
        <div :class="[!isSidebar ? '' : 'active']"></div>
      </button>
      <h1 class="text-sm md:text-[2.5rem] md:p-2">Higsby's Clearance Bin</h1>
    </div>

    <div class="flex">
      <div class="flex" :class="[!isSidebar ? 'slideIn' : ' slideOut']">
        <Sidebar :isSidebar="isSidebar" />
      </div>
      <!-- Content -->
      <div class="pt-16">
        <div>
          <draggable
            ghost-class="ghost"
            group="mods"
            :list="mods"
            @change="handleCheckout"
            @start="handleDragStart"
            @end="handleDrop"
            class="available_bin"
          >
            <chip-widget
              v-for="(mod, idx) in mods"
              :key="idx"
              :type="randomType()"
              :style="animationOrder(idx)"
            />
          </draggable>
        </div>
      </div>
    </div>
    <div class="shopping_bin">
      <div class="msg_box_dock top-[-40px]">Message Box</div>

      <draggable
        ghost-class="ghost"
        group="mods"
        :list="checkout"
        @change="handleRestore"
        @start="handleDragStart"
        class="bin"
        :class="dropAnim"
      >
        <chip-widget v-for="(mod, idx) in checkout" :key="idx" />
      </draggable>

      <button class="download_btn">Download</button>
    </div>
  </div>
</template>

<script>
import ChipWidget from "./components/ChipWidget.vue";
import draggable from "vuedraggable";
import Sidebar from "./components/Sidebar.vue";

export default {
  name: "App",
  components: {
    ChipWidget,
    draggable,
    Sidebar,
  },

  data() {
    return {
      mods: Array(10),
      checkout: [],
      sfx: {
        drop: new Audio(require("@/assets/sounds/drop.mp3")),
        pickup: new Audio(require("@/assets/sounds/pickup.mp3")),
      },
      dropAnim: "",
      isSidebar: false,
    };
  },

  methods: {
    handleCheckout(evt) {
      this.checkout.push(evt.object);
      this.sfx.drop.play();
    },

    handleRestore(evt) {
      this.mods.push(evt.object);
      this.sfx.drop.play();
    },

    handleDragStart() {
      this.sfx.pickup.play();
    },

    handleDrop() {
      console.log("Dropped");
      this.dropAnim = "drop_anim";
      const vm = this;
      setTimeout(1000, () => {
        vm.dropAnim = "";
      }); // reset the variable after 1000 ms (1 second)
    },

    randomType() {
      const types = ["Giga", "Mega", "Dark", "Standard"];
      return types[Math.floor(Math.random() * types.length)];
    },

    toggle() {
      this.isSidebar = !this.isSidebar;
    },
    animationOrder(idx) {
      return "--animation-order: " + idx + ";";
    },
  },
};
</script>

<style>
.slideIn {
  transform: translateX(-300px);
  transition: all 500ms ease-in-out;
  display: none;
  margin-right: -150px;
}

.slideOut {
  transform: translateX(0);
  transition: all 500ms ease-in-out;
  display: block;
  margin-right: 10px;
}

.hman_comeIn {
  animation: comeIn 3s;
}

@keyframes comeIn {
  0% {
    left: -1000px;
  }
  100% {
    left: -100px;
  }
}
.msg_box_slideIn {
  animation: msg_slideIn 2s;
}

@keyframes msg_slideIn {
  0% {
    opacity: 0;
    transform: translateX(-200px);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
