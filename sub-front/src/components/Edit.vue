<template>
  <div class="pt-5">
    <form @submit.prevent="update" method="post">
      <div class="form-group">
        <label for="name">Название</label>
        <input
          type="text"
          class="form-control"
          id="name"
          v-model="subscription.name"
          v-validate="'required'"
          name="name"
          placeholder="Введите название"
          :class="{
            'is-invalid': errors.has('subscription.name') && submitted,
          }"
        />
        <div class="invalid-feedback">
          Пожалуйста, введите правильно название.
        </div>
      </div>
      <div class="form-group">
        <label for="cost">Стоимость</label>
        <input
          type="number"
          name="cost"
          v-validate="'required'"
          class="form-control"
          id="cost"
          v-model="subscription.cost"
          placeholder="Введите цену"
          :class="{
            'is-invalid': errors.has('subscription.cost') && submitted,
          }"
        />
        <div class="invalid-feedback">Пожалуйста, введите правильно цену.</div>
      </div>
      <div class="form-group">
        <label for="description">Описание</label>
        <textarea
          name="description"
          class="form-control"
          id="description"
          v-validate="'required'"
          v-model="subscription.description"
          cols="30"
          rows="2"
          :class="{
            'is-invalid': errors.has('subscription.description') && submitted,
          }"
        ></textarea>
        <div class="invalid-feedback">
          Пожалуйста, введите правильно описание.
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Сохранить</button>
    </form>
  </div>
</template>
<script>

import axios from "axios";

export default {
  data() {
    return {
      subscription: {
        name: "",
        cost: "",
        description: "",
      },
      submitted: false,
    };
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/subscriptions/" + this.$route.params.id)
      .then((response) => {
        console.log(response.data);
        this.subscription = response.data;
      });
  },
  methods: {
    update: function (e) {
      this.$validator.validate().then((result) => {
        this.submitted = true;
        if (!result) {
          return;
        }
        axios
          .put(
            `http://127.0.0.1:8000/api/subscriptions/${this.subscription.pk}/`,
            this.subscription
          )
          .then((response) => {
            this.$router.push("/");
          });
      });
    },
  },
};
</script>
