<script>
import AddIcon from './icons/AddIcon.vue'
import EditIcon from './icons/EditIcon.vue'
import DeleteIcon from './icons/DeleteIcon.vue'
import AddEmployeeModal from './AddEmployeeModal.vue'

let locStore = ''

export default {
  components: {
    AddIcon: AddIcon,
    EditIcon: EditIcon,
    DeleteIcon: DeleteIcon,
    AddEmployeeModal: AddEmployeeModal
  },

  data() {
    return {
      headerList: ['id', 'name', 'location', 'email', 'phone'],
      empOrg: {},
      active: false,
      userData: {
        id: null,
        name: '',
        location: '',
        email: '',
        phone: null
      },
      validateMsg: {},
      enableDelete: true
    }
  },

  watch: {
    'userData.id': function (currVal) {
      ;(this.userData.id = currVal), this.validateId(currVal)
    },

    'userData.name': function (currVal) {
      ;(this.userData.name = currVal), this.validateName(currVal)
    },

    'userData.location': function (currVal) {
      ;(this.userData.location = currVal), this.validateLocation(currVal)
    },

    'userData.email': function (currVal) {
      ;(this.userData.email = currVal), this.validateEmail(currVal)
    },

    'userData.phone': function (currVal) {
      ;(this.userData.phone = currVal), this.validatePhone(currVal)
    }
  },
  computed: {
    enableSubmitBtn() {
      if(this.validateMsg["id"] == '' && 
        this.validateMsg["name"] == '' && 
        this.validateMsg["location"] == '' && 
        this.validateMsg["email"] == '' && 
        this.validateMsg["phone"] == ''
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
  methods: {
    addOrEditUser() {
      if (this.userData != null && this.enableSubmitBtn) {
        if (this.empOrg[this.userData.id]) {
          this.empOrg[this.userData.id] = this.userData
        } else {
          this.empOrg[this.userData.id] = this.userData
        }
        localStorage.setItem(this.userData.id, JSON.stringify(this.userData))
      }
    },

    addOrEditBtnClick(emp) {
      if (emp != null) {
        this.userData = JSON.parse(JSON.stringify(emp))
        this.enableDelete = true;
        if(emp.id == null) {
          this.enableDelete = false;
        }
      }
      this.active = true
    },

    deleteUserClick() {
      if (this.userData != null && this.empOrg[this.userData.id]) {
        delete this.empOrg[this.userData.id]
        localStorage.removeItem(this.userData.id)
      }
    },

    validateId(id) {
      if (id != null && id != '') {
        this.validateMsg['id'] = ''
      } else {
        this.validateMsg['id'] = 'Please enter employee id'
      }
    },

    validateName(name) {
      if (name != null && name != '') {
        this.validateMsg['name'] = ''
      } else {
        this.validateMsg['name'] = 'Please enter name'
      }
    },

    validateLocation(location) {
      if (location != null && location != '') {
        this.validateMsg['location'] = ''
      } else {
        this.validateMsg['location'] = 'Please enter user location'
      }
    },

    validateEmail(email) {
      if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(email)) {
        this.validateMsg['email'] = ''
      } else {
        this.validateMsg['email'] = 'Please enter valid email'
      }
    },

    validatePhone(phone) {
      if (/^\d{10}$/.test(phone)) {
        this.validateMsg['phone'] = ''
      } else {
        this.validateMsg['phone'] = 'Please enter 10 digit phone number'
      }
    }
  },
  mounted() {
    locStore = JSON.parse(JSON.stringify(localStorage))
    Object.keys(locStore).map((key) => (this.empOrg[key] = JSON.parse(locStore[key])))
  }
}
</script>

<template>
  <div class="container">
    <h1 class="title">Employee Directory</h1>
    <div class="emp-list">
      <div class="list-header">
        <h2>Employee list</h2>
        <i class="icon-container" @click="addOrEditBtnClick">
          <AddIcon />
        </i>
      </div>

      <div class="table-container">
        <table>
          <thead>
            <tr>
              <th v-for="head in headerList" :key="head">
                {{ head }}
              </th>
              <th>Modify</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="empId in Object.keys(empOrg)" :key="empId">
              <td v-for="key in headerList" :key="key">{{ empOrg[empId][key] }}</td>
              <td>
                <div class="icon-container">
                  <i @click="addOrEditBtnClick(empOrg[empId])">
                    <EditIcon />
                  </i>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <AddEmployeeModal v-if="active" @close="active = false">
      <template #fields>
        <div class="form-group">
          <label for="id">Employee Id</label>
          <input type="number" class="form-control" v-model="userData.id" required />
          <span v-if="validateMsg.id" class="error">{{ validateMsg.id }}</span>
        </div>
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" class="form-control" v-model="userData.name" required />
          <span v-if="validateMsg.name" class="error">{{ validateMsg.name }}</span>
        </div>
        <div class="form-group">
          <label for="location">Location</label>
          <input type="text" class="form-control" v-model="userData.location" required />
          <span v-if="validateMsg.location" class="error">{{ validateMsg.location }}</span>
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input type="text" id="email" class="form-control" v-model="userData.email" required />
          <span v-if="validateMsg.email" class="error">{{ validateMsg.email }}</span>
        </div>
        <div class="form-group">
          <label for="phone">Phone Number</label>
          <input type="number" class="form-control" v-model="userData.phone" required />
          <span v-if="validateMsg.phone" class="error">{{ validateMsg.phone }}</span>
        </div>
      </template>
      <template #submit>
        <button v-if="enableSubmitBtn" @click="addOrEditUser">Add/Edit</button>
        <i v-if="enableDelete" @click="deleteUserClick(empId)">
          <DeleteIcon />
        </i>
      </template>
    </AddEmployeeModal>
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
}

.title {
  text-align: center;
}

.list-header {
  display: flex;
  justify-content: space-between;
  padding: 10px 30px;
  margin: 10px;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;
  cursor: pointer;
}

.icon-container {
  display: flex;
  justify-content: space-around;
  align-self: center;
}

.table-container {
  margin: 0 30px;
}

table {
  width: 100%;
  border: 2px solid lightskyblue;
  border-radius: 3px;
  background-color: #fff;
}

th {
  background-color: lightskyblue;
  color: black;
}

td {
  background-color: #f9f9f9;
}

th,
td {
  min-width: 120px;
  padding: 10px 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding-bottom: 20px;
}

.error {
  color: lightcoral;
  font-size: 10px;
}

button {
  background-color: lightgreen;
  border-radius: 5px;
  border: 1px solid black;
  padding: 5px;
  cursor: pointer;
}
</style>
