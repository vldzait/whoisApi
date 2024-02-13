<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <v-form ref="form">
        <v-row class="d-flex align-center justify-center">
          <v-col cols="4">
            <v-select
              v-model="selectedRequestType"
              :items="requestTypeList"
              :rules="[v => !!v || 'Item is required']"
              label="Выберите тип запроса"
              required
            ></v-select>
          </v-col>
          <v-col cols="5">
            <v-text-field
              v-model="url"
              :rules="urlRules"
              label="Введите url сайта"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="3">
            <v-btn
              color="success"
              block
              @click="validate"
            >
              Отправить
            </v-btn>
          </v-col>
        </v-row>
      </v-form>

      <v-table>
        <thead>
          <tr>
            <th class="text-left">
              Name
            </th>
            <th class="text-left">
              Calories
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(value, key, index) in info"
            :key="index"
          >
            <td><b>{{ key }}</b></td>
              <td v-if="key==='isp_url'">
                <a :href="value" target="blank">
                  {{ value }}
                </a>
              </td>
            <td v-else>{{ value }}</td>
          </tr>
        </tbody>
      </v-table>

    </v-responsive>
  </v-container>
</template>

<script>
  import axios from "axios";

  export default {
    data: () => ({
      urlRules: [
        v => !!v || 'Поле url сайта обязательно',
      ],
      selectedRequestType: null,
      requestTypeList: [
        {
          title: 'Данные хостинга',
          value: 'Host'
        },
        {
          title: 'Технологии',
          value: 'Tech'
        }
      ],
      info: {},
      key: 'wpyymclwlkwb2ietq90vh8e4m8lnjq99fovzd0x8i2908cddnqouselg5sicb4bnfmxih3', // TODO: засунуть в env
      url: ''
    }),
    
    methods: {
      async validate () {
        const { valid } = await this.$refs.form.validate()

        if (valid) {
          this.getAnswer();
        }
      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },

      async getAnswer() {
        const { data } = await axios.get(`https://www.who-hosts-this.com/API/${this.selectedRequestType}?key=${this.key}&url=${this.url}`);
        console.log(data.results[1]) // TODO: delete
        this.info = data.results[1];
      },
    },
  }
</script>

