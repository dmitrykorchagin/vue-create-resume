<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent>
      <app-select v-model="selectValue"></app-select>
      <app-textarea v-model="fieldValue" label="Значение"></app-textarea>
      <app-button
        color="primary"
        :disabled="fieldValue.length <= 3"
        @click="addField"
      >
        Добавить
      </app-button>
    </form>

    <div class="card card-w70">
      <template v-if="fields.length > 0">
        <component
          v-for="field in fields"
          :key="field.id"
          :is="`app-${field.type}`"
          v-bind="{ value: field.value }"
          @delete="deleteField(field.id)"
          @update-value="value => updateField(value, field.id)"
        ></component>
      </template>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>

  <div class="container">
    <p>
      <app-button color="primary" @click="loadComment">
        Загрузить комментарии
      </app-button>
    </p>
    <app-comments
      :comments="comments"
      v-if="comments.length > 0"
    ></app-comments>
    <app-loader v-if="loading"></app-loader>
  </div>
</template>

<script>
import axios from 'axios'

import AppSelect from './components/UI/AppSelect.vue'
import AppTextarea from './components/UI/AppTextarea.vue'
import AppTitle from './components/UI/AppTitle.vue'
import AppSubtitle from './components/UI/AppSubtitle.vue'
import AppText from './components/UI/AppText.vue'
import AppAvatar from './components/UI/AppAvatar.vue'
import AppComments from './components/UI/AppComments.vue'
import AppLoader from './components/UI/AppLoader.vue'
import AppButton from './components/UI/AppButton.vue'

export default {
  data() {
    return {
      comments: [],
      loading: false,
      selectValue: 'title',
      fieldValue: '',
      fields: []
    }
  },
  components: {
    AppSelect,
    AppTextarea,
    AppTitle,
    AppSubtitle,
    AppText,
    AppAvatar,
    AppComments,
    AppLoader,
    AppButton
  },
  methods: {
    async loadComment() {
      try {
        this.loading = true
        const { data } = await axios.get(
          'https://jsonplaceholder.typicode.com/comments?_limit=42'
        )
        this.comments = data
      } catch (error) {
        this.loading = false
        console.log(error)
      }
      this.loading = false
    },
    async addField() {
      try {
        const {
          data
        } = await axios.post(
          'https://create-resume-d318f-default-rtdb.firebaseio.com/resume.json',
          { type: this.selectValue, value: this.fieldValue }
        )

        this.fields.push({
          id: data.name,
          type: this.selectValue,
          value: this.fieldValue
        })

        this.fieldValue = ''
        this.selectValue = 'title'
      } catch (error) {
        console.log(error)
      }
    },
    async loadResume() {
      try {
        const { data } = await axios.get(
          'https://create-resume-d318f-default-rtdb.firebaseio.com/resume.json'
        )

        if (data) {
          this.fields = Object.keys(data).map(key => {
            return {
              id: key,
              ...data[key]
            }
          })
        }
      } catch (error) {
        console.log(error)
      }
    },
    async deleteField(id) {
      try {
        await axios.delete(
          `https://create-resume-d318f-default-rtdb.firebaseio.com/resume/${id}.json`
        )
        this.fields = this.fields.filter(i => i.id !== id)
      } catch (error) {
        console.log(error)
      }
    },
    async updateField(value, id) {
      if (value) {
        try {
          await axios.patch(
            `https://create-resume-d318f-default-rtdb.firebaseio.com/resume/${id}.json`,
            { value: value }
          )
        } catch (error) {
          console.log(error)
        }
      } else {
        this.deleteField(id)
      }
    }
  },
  mounted() {
    this.loadResume()
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}
</style>
