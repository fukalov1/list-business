<template>
  <div id="app">

    <confirm-form v-if="confirm" :data="oneBusiness" v-on:delete-data="deleteData()" v-on:set-cancel="setCancel()"/>

    <nav class="navbar navbar-expand-md navbar-dark bg-dark">
      <a class="navbar-brand" href="#">Мои дела</a>

        <ul class="navbar-nav mr-auto">
          <li class="nav-item" :class="{active: currentMenu==='list'}">
            <a class="nav-link"  @click="mode='';currentMenu='list'">Список</a>
          </li>
          <li class="nav-item" :class="{active: currentMenu==='form'}">
            <a class="nav-link" @click="addData()">Новое дело</a>
          </li>
          <li class="nav-item" :class="{active: currentMenu==='clear'}">
             <a class="nav-link" @click="clearStorage()">Новый список</a>
          </li>
        </ul>
    </nav>

    <div class="row">
      <div class="col-12">
        <div class="row" v-if="mode===''">
          <div class="col-12">
            <span class="h5">Список дел</span>
            <table class="table table-striped">
              <thead>
              <tr>
                <th>Время</th>
                <th>Наименование</th>
                <th class="col-hide">Подробности</th>
                <th>Выполнено</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(item, index) in listBusiness"
                  :key="index"
                  @click="editData(item, index)">
                <td>{{ item.time }}</td>
                <td>{{ item.name }}</td>
                <td class="col-hide">{{ item.text }}</td>
                <td>{{ showStatus(item.exec) }}</td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="row" v-else>
          <div class="col-xl-4 col-lg-4 col-md-12 col-12"></div>
          <div class="col-xl-4 col-lg-4 col-md-12 col-12 text-center">
            <form-business
                    :oneBusiness="oneBusiness"
                    :currentItem="currentItem"
                    v-on:set-cancel="mode=''"
                    v-on:confirm-delete="confirmDelete()"
                    v-on:save-data="saveData()">Новое дело</form-business>
          </div>
          <div class="col-xl-4 col-lg-4 col-md-12 col-12"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import FormBusiness from './components/FormBusines.vue'
  import ConfirmForm from "@/components/ConfirmForm";


  export default {
    name: 'app',
    components: {
      FormBusiness, ConfirmForm
    },
    data() {
      return {
        listBusiness: [],
        oneBusiness: {'time': '', 'name': '', 'text': '', 'exec': false},
        mode: '',
        currentItem: null,
        confirm: false,
        currentMenu: 'list'
      }
    },
    mounted() {
      this.getData();
    },
    computed: {
      sortList: function () {
        let obj = this.listBusiness;
        obj.sort((a, b) => (parseInt(a.time) < parseInt(b.time)) ? 1 : ((parseInt(b.time) < parseInt(a.time)) ? -1 : 0));
        return obj;
      }
    },
    methods: {
      showStatus($data) {
        return $data ? 'выполнено' : 'не выполнено';
      },
      getData() {
        if (localStorage.getItem('listBusiness')) {
          try {
            this.listBusiness = JSON.parse(localStorage.getItem('listBusiness'));
          }
          catch(e) {
            localStorage.removeItem('listBusiness');
          }
        }
        else {
          this.listBusiness = [];
        }
      },
      clearStorage() {
        localStorage.removeItem('listBusiness');
        this.getData();
        this.currentMenu='clear';
      },
      saveStorage() {
        let parsed = JSON.stringify(this.listBusiness);
        localStorage.setItem('listBusiness', parsed);
      },
      addData() {
        this.oneBusiness = {'time': '', 'name': '', 'text': '', 'exec': false};
        this.mode = 'add';
        this.currentMenu='form';
      },
      editData($item, $index) {
        this.oneBusiness = $item;
        this.currentItem = $index;
        this.mode = 'edit';
        this.currentMenu='form';
      },
      saveData() {
        if (this.mode === 'add') {
          this.listBusiness.push(this.oneBusiness);
        }
        this.saveStorage();
        this.mode='';
      },
      confirmDelete() {
        this.confirm = true;
      },
      deleteData() {
        this.confirm = false;
        this.listBusiness.splice(this.currentItem,1);
        this.saveStorage();
        this.mode = '';
      },
      setCancel() {
        this.confirm = false;
        this.mode = '';
      },
    }
  }
</script>


