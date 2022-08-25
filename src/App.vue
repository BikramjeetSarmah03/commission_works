<!-- As it's using tailwind, u can install the tailwind Css Intellisense from the extensions, and just hover over the classes to see the css, btw some classes are used for custom css also so there it will not show their css -->

<template>
  <div id="app">
    <!-- Side Man Image -->
    <div class="fixed bottom-0 z-50">
      <img
        src="@/assets/images/hman.png"
        alt="hman"
        class="fixed bottom-0 -scale-x-100 translate-y-[400px] left-[-100px] hman_comeIn"
      />
      <!-- hman_comeIn  -->
      <!-- By default it's hidden -->
      <div class="hidden">
        <div class="msg_box_inner -rotate-4 clip_message">
          <div class="-mt-5 rotate-4">Message</div>
        </div>
      </div>
    </div>

    <!-- Navbar -->
    <div class="header">
      <button
        class="absolute text-lg cursor-pointer left-5 top-2"
        @click.prevent="toggle()"
      >
        X
      </button>
      <h1>Higsby's Clearance Bin</h1>
    </div>

    <div class="flex">
      <div class="flex sidebar" :class="[!open ? 'slideIn' : ' slideOut']">
        <Sidebar />
      </div>
      <!-- Content -->
      <div class="px-[15px] pt-16">
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
      open: false,
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
      this.open = !this.open;
      console.log(this.open);
      console.log("slidebar");
    },
    animationOrder(idx) {
      return "--animation-order: " + idx + ";";
    },
  },
};
</script>

<style>
.slideIn {
  transform: translateX(-150px);
  transition: all 2s ease-in-out;
}

.slideOut {
  transform: translateX(0);
  transition: all 2s ease-in-out;
}

.hman_comeIn {
  animation: comeIn 3s;
}

.clip_message {
  clip-path: polygon(
    33% 34%,
    100% 17%,
    100% 79%,
    32% 58%,
    26% 70%,
    18% 57%,
    12% 63%,
    1% 52%,
    13% 51%,
    15% 43%,
    26% 47%
  );
}

@keyframes comeIn {
  0% {
    left: -1000px;
  }
  100% {
    left: -100px;
  }
}
</style>
