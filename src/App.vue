<template>
  <div class="container">
    <div class="center">
      <div class="columns">
        <div class="column">
          <h1 class="text-center">Guest a Number!</h1>
          <p v-if="status != null" class="text-center">{{ status }}</p>
        </div>
      </div>
      <div class="columns">
        <div class="column ">
          <div class="field">
            <div class="control">
              <input
                class="input is-primary"
                type="text"
                placeholder="Input Number"
                v-model="user_number"
              />
            </div>
          </div>
        </div>
      </div>
      <div class="columns" v-if="is_start">
        <div class="column">
          <button @click="guest" class="button is-primary">Guest</button>
        </div>
        <div class="column">
          <button @click="hint" class="button is-warning">Hint</button>
        </div>
      </div>
      <div class="columns" v-else>
        <div class="column">
          <button @click="startGame" class="button is-dark">Start</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
.container {
  height: 100vh;
  position: relative;
}

.center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 300px;
}

.number-container {
  border-radius: 5px;
}

.text-center {
  text-align: center;
}

.button {
  width: 100%;
}
</style>

<script>
export default {
  data() {
    return {
      user_number: null,
      final_number: 0,
      is_start: false,
      guest_chance: 0,
      hint_change: 0,
      status: null,
    }
  },
  methods: {
    startGame() {
      this.is_start = true
      this.final_number = Math.floor(Math.random() * 10) + 1
      this.guest_chance = 3
      this.hint_change = 1
      this.status = null
      this.user_number = null
    },
    guest() {
      if (this.validation()) {
        this.guest_chance--
        this.check()
      }
    },
    hint() {
      if (this.validation()) {
        if (this.hint_change == 0) {
          alert("You dont have hint anymore")
        } else {
          this.hint_change--
          if (this.user_number < this.final_number) {
            this.status = "Grater than " + this.user_number
          } else {
            this.status = "Lowers than " + this.user_number
          }
        }
      }
    },
    check() {
      if (this.user_number == this.final_number) {
        this.status = "You win! the answer is " + this.final_number
        this.is_start = false
        return
      } else {
        this.status = `You wrong! you have ${this.guest_chance} guest chance`
      }

      if (this.guest_chance == 0) {
        this.is_start = false
        this.status = "You lost! the answer is " + this.final_number
      }
    },
    validation() {
      if (this.user_number == null) {
        alert("Insert number!")
        return false
      } else if (isNaN(this.user_number)) {
        alert("Insert number!")
        return false
      } else {
        return true
      }
    },
  },
  watch: {
    user_number() {
      if (this.is_start == false) {
        alert("Press start!")
      }
    },
  },
}
</script>
