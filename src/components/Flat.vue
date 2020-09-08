<template>
  <div class="flat">
    <h2>{{ flat.name }}</h2>
    <p>{{ flat.address }}</p>
    <p>{{ flat.city }}</p>
    <span class="toggle-favorite pointer" @click="toggle(flat.id)">
      <Icon
        class="toggle-favorite__icon"
        :class="iconClasses"
        @animationend.native="onIconAnimationEnds"
      />
    </span>
  </div>
</template>

<script>
import Icon from "../components/Icon";
export default {
  components: {
    Icon,
  },
  props: ['flat'],
  data() {
    return {
      favorited: this.flat.fav,
      animating: false,
    };
  },
  computed: {
    iconClasses() {
      return {
        "toggle-favorite__icon--favorited": this.favorited,
        "toggle-favorite__icon--animate": this.animating,
      };
    },
  },
  methods: {
    likeFlat() {},
    toggle() {
      if (!this.favorited) {
        this.animating = true;
      }

      this.favorited = !this.favorited
      this.$emit('flat-fav-toggled', this.flat.id, this.favorited)
    },
    onIconAnimationEnds() {
      this.animating = false;
    },
  },
};
</script>
