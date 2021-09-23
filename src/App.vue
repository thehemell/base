<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="table-responsive">
          <table class="table mb-4">
          <thead class="thead-dark">
          <tr>
            <th scope="col" @click="peopleSort('id')">#</th>
            <th scope="col" @click="peopleSort('name')">ФИО</th>
            <th scope="col" @click="peopleSort('salary')">Зарплата</th>
            <th scope="col" @click="peopleSort('role')">Должность</th>
            <th scope="col" @click="peopleSort('phone')">Телефон</th>
            <th scope="col" @click="peopleSort('birthday')">Дата рождения</th>
          </tr>
          </thead>
          <tbody>
            <tr v-for="person in listPeople" :key="person.id">
              <th scope="row">{{ person.id }}</th>
              <td>{{ person.name }}</td>
              <td>{{ person.salary }}</td>
              <td>{{ person.role }}</td>
              <td>{{ person.phone }}</td>
              <td>{{ person.birthday }}</td>
            </tr>
          </tbody>
        </table>
        </div>
        <div class="text-right mb-4">Итого: <strong>{{ totalSalary }}</strong></div>
        <div class="card">
          <div class="card-body">
            <div class="form-group mb-2">
              <label for="inputName">ФИО</label>
              <input v-model="input.name" type="text" class="form-control" id="inputName" placeholder="Иван Иванов Иванович">
              <div class="invalid-feedback" :class="{ 'd-block': isError('name') }">
                Поле должно быть заполнено
              </div>
            </div>
            <div class="form-group mb-2">
              <label for="inputSalary">Зарплата</label>
              <input v-model="input.salary" type="number" class="form-control" id="inputSalary" placeholder="10000">
              <div class="invalid-feedback" :class="{ 'd-block': isError('salary') }">
                Поле должно быть заполнено
              </div>
            </div>
            <div class="form-group mb-2">
              <label for="inputPlace">Должность</label>
              <input v-model="input.role" type="text" class="form-control" id="inputPlace" placeholder="Сотрудник">
              <div class="invalid-feedback" :class="{ 'd-block': isError('role') }">
                Поле должно быть заполнено
              </div>
            </div>
            <div class="form-group mb-2">
              <label for="inputPhone">Телефон</label>
              <input v-model="input.phone" type="tel" class="form-control" id="inputPhone" placeholder="+7 (883) 508-3269" v-mask="'+# (###) ###-####'">
              <div class="invalid-feedback" :class="{ 'd-block': isError('phone') }">
                Поле должно быть заполнено
              </div>
            </div>
            <div class="form-group mb-2">
              <label for="inputBirthday">Дата рождения</label>
              <input v-model="input.birthday" type="date" class="form-control" id="inputBirthday" placeholder="Сотрудник">
              <div class="invalid-feedback" :class="{ 'd-block': isError('birthday') }">
                Поле должно быть заполнено
              </div>
            </div>
            <button type="submit" class="btn btn-primary" @click="addPerson" :disabled="!allInputs">Добавить</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import {mask} from 'vue-the-mask'

export default {
  name: 'App',
  directives: {mask},
  data: () => ({
    people: [
      {
        "id": 1,
        "name": "Илья Емельянов",
        "salary": 30000,
        "role": "developer",
        "phone": "+7 (883) 508-3269",
        "birthday": "12.02.1982"
      },
      {
        "id": 2,
        "name": "Александр Ларионов",
        "salary": 35000,
        "role": "manager",
        "phone": "+7 (823) 440-3602",
        "birthday": "26.01.1986"
      },
      {
        "id": 3,
        "name": "Богдан Давыдов",
        "salary": 5000,
        "role": "developer",
        "phone": "+7 (971) 575-2645",
        "birthday": "29.11.1990"
      },
      {
        "id": 4,
        "name": "Олимпиада Макарова",
        "salary": 33000,
        "role": "manager",
        "phone": "+7 (945) 447-2286",
        "birthday": "06.01.1987"
      },
      {
        "id": 5,
        "name": "Алла Котова",
        "salary": 25000,
        "role": "designer",
        "phone": "+7 (948) 523-2964",
        "birthday": "26.01.1982"
      },
      {
        "id": 6,
        "name": "Кира Колесникова",
        "salary": 46000,
        "role": "designer",
        "phone": "+7 (929) 592-3637",
        "birthday": "25.02.1972"
      },
      {
        "id": 7,
        "name": "Александр Третьяков",
        "salary": 51000,
        "role": "developer",
        "phone": "+7 (872) 568-2916",
        "birthday": "31.05.1979"
      },
      {
        "id": 8,
        "name": "Пелагея Морозова",
        "salary": 8000,
        "role": "developer",
        "phone": "+7 (977) 521-3479",
        "birthday": "11.09.1981"
      },
      {
        "id": 9,
        "name": "Агафон Громов",
        "salary": 34000,
        "role": "developer",
        "phone": "+7 (868) 569-3159",
        "birthday": "07.06.1988"
      },
      {
        "id": 10,
        "name": "Владлен Тетерин",
        "salary": 15000,
        "role": "developer",
        "phone": "+7 (808) 592-2480",
        "birthday": "20.06.1978"
      },
      {
        "id": 11,
        "name": "Валерий Пестов",
        "salary": 30500,
        "role": "designer",
        "phone": "+7 (899) 403-2387",
        "birthday": "20.01.1987"
      },
      {
        "id": 12,
        "name": "Даниил Кузнецов",
        "salary": 26060,
        "role": "manager",
        "phone": "+7 (933) 582-2673",
        "birthday": "25.05.1987"
      },
      {
        "id": 13,
        "name": "Фёдор Веселов",
        "salary": 33005,
        "role": "manager",
        "phone": "+7 (951) 517-3787",
        "birthday": "16.12.1972"
      },
      {
        "id": 14,
        "name": "Пантелеймон Ефимов",
        "salary": 39000,
        "role": "designer",
        "phone": "+7 (807) 492-3627",
        "birthday": "17.04.1986"
      },
      {
        "id": 15,
        "name": "Иванна Калашникова",
        "salary": 25000,
        "role": "manager",
        "phone": "+7 (927) 488-2568",
        "birthday": "24.03.1982"
      },
      {
        "id": 16,
        "name": "Прасковья Кондратьева",
        "salary": 16000,
        "role": "designer",
        "phone": "+7 (875) 517-3873",
        "birthday": "07.06.1983"
      },
      {
        "id": 17,
        "name": "Евдокия Филиппова",
        "salary": 14000,
        "role": "manager",
        "phone": "+7 (877) 450-3253",
        "birthday": "03.12.1994"
      }
    ],
    input: {
      name: null,
      salary: null,
      phone: null,
      role: null,
      birthday: null,
    },
    sort: 'default',
    type: 'low'
  }),
  methods: {
    addPerson() {
      if (this.allInputs) {
        let date = this.input.birthday.split('-')

        this.people.push({
          id: this.people.length + 1,
          name: this.input.name,
          salary: Number(this.input.salary),
          phone: this.input.phone,
          role: this.input.role,
          birthday: `${date[2]}.${date[1]}.${date[0]}`
        })
      }
    },
    peopleSort(sort) {
      console.log(this.sort, sort, this.sort === sort)

      this.type = this.sort === sort && this.type === 'high' ? 'low' : 'high'
      this.sort = sort
    },
    isError(type) {
      if (type === 'phone' && this.input.phone !== null) {
        return this.input.phone.length !== 17
      }

      return this.input[type] === null ? false : !this.input[type]
    }
  },
  computed: {
    totalSalary() {
      let total = 0

      this.people.forEach(person => {
        total = Number(total) + Number(person.salary)
      })

      return total
    },
    listPeople() {
      const sortKey = this.sort
      let people = [...this.people]

      if (this.type === 'low') {
        people = people.sort(function (a, b) {
          if (a[sortKey] > b[sortKey]) {
            return 1
          }

          if (a[sortKey] < b[sortKey]) {
            return -1
          }

          return 0
        })

        return people
      } else {
        people = people.sort(function (a, b) {
          console.log(a[sortKey], b[sortKey], a[sortKey] > b[sortKey])
          if (a[sortKey] > b[sortKey]) {
            return -1
          }

          if (a[sortKey] < b[sortKey]) {
            return 1
          }

          return 0
        })

        return people
      }
    },
    allInputs() {
      let input = false

      for (const [key, value] of Object.entries(this.input)) {
        console.log(`${key}: ${value}`);

        if (value === null) {
          return false
        }

        if (value === '') {
          return false
        }

        input = true
      }

      return input
    }
  }
}
</script>

<style>
@import '~bootstrap/dist/css/bootstrap.css';

th {
  cursor: pointer;
}
</style>
