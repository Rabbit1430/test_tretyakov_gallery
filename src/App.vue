<!-- @format -->

<template>
  <div class="project">
    <div>
      <h2>Таблица</h2>
      <table>
        <thead>
          <tr>
            <th>Фамилия</th>
            <th>Имя</th>
            <th>Отчество</th>
          </tr>
        </thead>
        <tbody class="tbody">
          <tr
            :style="{
              cursor: 'pointer',
              'background-color':
                user.id === userSelect ? '#eaeaea' : 'transparent',
            }"
            v-for="user in users"
            :key="user.id"
            @click="userSelectid(user.id)"
          >
            <td>{{ user.familya }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.surname }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="inpt">
      <h2>Поля</h2>
      <div class="input_all">
        <div>
          <ui-input
            type="text"
            placeholder="Введите Фамилию"
            v-model="people.familya"
          />
        </div>
        <div>
          <ui-input
            type="text"
            placeholder="Введите Имя"
            v-model="people.name"
          />
        </div>
        <div>
          <ui-input
            type="text"
            placeholder="Введите Очество"
            v-model="people.surname"
          />
        </div>
      </div>
      <div class="btn_all">
        <ui-button @click="addUser">Добавить</ui-button>
        <ui-button @click="updateUser">Изменить</ui-button>
        <ui-button @click="deleteUser">Удалить</ui-button>
      </div>
      <div>
        <h1
          :style="{
            color: 'red',
          }"
        >
          {{ err }}
        </h1>
      </div>
    </div>
<set-kvadrat/>
  </div>
</template>

<script>
import UiButton from "@/components/UI/UiButton.vue";
import UiInput from "@/components/UI/UiInput.vue";
import SetKvadrat from './components/SetKvadrat.vue';

export default {
  components: { UiButton, UiInput, SetKvadrat },

  data() {
    return {
      // задание 2
      people: {
        familya: "",
        name: "",
        surname: "",
      },
      userSelect: null,
      err: "",
      userselecteds: {},

      users: [],
    };
  },

  methods: {
    //1 задание

    async sendpost(url) {
      const response = await fetch(url, {
        method: "POST",
      });
      if (response.ok) {
        const data = await response.text();
        console.log("Данные с сервака:", data);
      } else {
        throw new Error("Ошибка в запросе на сервак");
      }
    },

    //задание 2

    async getUser() {
      const urlget = "http://localhost:3000/users";
      try {
        const response = await fetch(urlget);
        const data = await response.json();
        this.users = data;
      } catch (error) {
        this.err = error.message;
      }
    },

    async addUser() {
      if (!this.people.familya || !this.people.name || !this.people.surname) {
    this.err = "Заполните все поля";
    return;
  }
      const adduser = "http://localhost:3000/users/add";
      try {
        const response = await fetch(adduser, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(this.people),
        });
        const data = await response.json();
        if (data.error) {
          // console.error(data.error);
          this.err = data.error;
        } else {
          this.err = "";
          this.getUser();
          this.people = {
            familya: "",
            name: "",
            surname: "",
          };
        }
        // console.log(data);
      } catch (error) {
        console.error(error);
      }
    },

    async updateUser() {
      if (!this.userSelect || !this.people.familya || !this.people.name || !this.people.surname) {
    this.err = "Выберите пользователя";
    return;
  }
      const updateuser = "http://localhost:3000/users/update";
      
      if (this.userSelect) {
        const user = {
          id: this.userSelect,
          familya: this.people.familya,
          name: this.people.name,
          surname: this.people.surname,
        };

        try {
          const response = await fetch(updateuser, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(user),
          });
          const data = await response.json();
          if (data.error) {
            // console.error(data.error);
            this.err = data.error;
          } else {
            this.err = "";
            this.getUser();
            this.userSelect = null
            this.people = {
              familya: "",
              name: "",
              surname: "",
            };
          }
        } catch (error) {
          console.error(error);
        }
      }
    },

    async deleteUser() {
      if (!this.userSelect) {
    this.err = "Выберите пользователя";
    return;
  }
      const deleteuser = `http://localhost:3000/users/delete/${this.userSelect}`;
      if (this.userSelect) {
        const user = this.userSelect;
        try {
          const response = await fetch(deleteuser, {
            method: "DELETE",
            headers: { "Content-Type": "application/json" },
          });
          const data = await response.json();
          if (data.error) {
            // console.error(data.error);
            this.err = data.error;
          } else {
            this.err = "";
            this.getUser();
            this.userSelect = null
            this.people = {
              familya: "",
              name: "",
              surname: "",
            };
          }
        } catch (error) {
          console.error(error);
        }
      }
    },

    userSelectid(id) {
      if (this.userSelect === id) {
        this.userSelect = null;
        this.userselecteds = {};
        this.people = {
          familya: "",
          name: "",
          surname: "",
        };
      } else {
        this.userSelect = id;
        this.userselecteds = this.users.find((user) => user.id === id);
        this.people = this.userselecteds;
      }
    },

    allSuccess() {
      alert("УРА!");
    },
  },

  async mounted() {
    this.getUser();
    try {
      await Promise.all([
        this.sendpost("http://localhost:3000/backend1"),
        this.sendpost("http://localhost:3000/backend2"),
        this.sendpost("http://localhost:3000/backend3"),
      ]);
      this.allSuccess();
    } catch (error) {
      console.error("Ошибка:", error.message);
    }
  },
};
</script>

<style scoped>

.project{
  width: 100%;
  margin-left: 10%;
 

}
.btn_all {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}
.input_all {
  display: flex;
  margin-top: 20px;
}

table {
  width: 70%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #000;
  padding: 8px;
  text-align: left;
}

.inpt {
  margin-top: 100px;
}


</style>
