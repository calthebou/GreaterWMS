<template>
    <div>
      <transition appear enter-active-class="animated fadeIn">
      <q-table
        class="my-sticky-header-column-table shadow-24"
        :data="table_list"
        row-key="id"
        :separator="separator"
        :loading="loading"
        :filter="filter"
        :columns="columns"
        hide-bottom
        :pagination.sync="pagination"
        no-data-label="No data"
        no-results-label="No data you want"
        :table-style="{ height: height }"
        flat
        bordered
      >
         <template v-slot:top>
           <q-btn-group push>
             <q-btn :label="$t('new')" icon="add" @click="newFormCheck()">
               <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                 {{ $t('newtip') }}
               </q-tooltip>
             </q-btn>
             <q-btn :label="$t('refresh')" icon="refresh" @click="reFresh()">
               <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                 {{ $t('refreshtip') }}
               </q-tooltip>
             </q-btn>
           </q-btn-group>
           <q-space />
         </template>
         <template v-slot:body="props">
           <q-tr :props="props">
             <template v-if="props.row.id === editid">
               <q-td key="warehouse_name" :props="props">
                 <q-input dense
                          outlined
                          square
                          v-model="editFormData.warehouse_name"
                          :label="$t('warehouse.view_warehouse.warehouse_name')"
                          autofocus
                          :rules="[ val => val && val.length > 0 || error1]"
                 />
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_name" :props="props">
                 {{ props.row.warehouse_name }}
               </q-td>
             </template>
             <template v-if="props.row.id === editid">
               <q-td key="warehouse_city" :props="props">
                 <q-input dense
                          outlined
                          square
                          v-model="editFormData.warehouse_city"
                          :label="$t('warehouse.view_warehouse.warehouse_city')"
                          :rules="[ val => val && val.length > 0 || error2]"
                 />
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_city" :props="props">
                 {{ props.row.warehouse_city }}
               </q-td>
             </template>
             <template v-if="props.row.id === editid">
               <q-td key="warehouse_address" :props="props">
                 <q-input dense
                          outlined
                          square
                          v-model="editFormData.warehouse_address"
                          :label="$t('warehouse.view_warehouse.warehouse_address')"
                          :rules="[ val => val && val.length > 0 || error3]"
                 />
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_address" :props="props">
                 {{ props.row.warehouse_address }}
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_contact" :props="props">
                 {{ props.row.warehouse_contact }}
               </q-td>
             </template>
             <template v-if="props.row.id === editid">
               <q-td key="warehouse_contact" :props="props">
                 <q-input dense
                          outlined
                          square
                          v-model="editFormData.warehouse_contact"
                          :label="$t('warehouse.view_warehouse.warehouse_contact')"
                          :rules="[ val => val && val.length > 0 || error4]"
                 />
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_contact" :props="props">
                 {{ props.row.warehouse_contact }}
               </q-td>
             </template>
             <template v-if="props.row.id === editid">
               <q-td key="warehouse_manager" :props="props">
                 <q-input dense
                          outlined
                          square
                          v-model="editFormData.warehouse_manager"
                          :label="$t('warehouse.view_warehouse.warehouse_manager')"
                          :rules="[ val => val && val.length > 0 || error5]"
                 />
               </q-td>
             </template>
             <template v-else-if="props.row.id !== editid">
               <q-td key="warehouse_manager" :props="props">
                 {{ props.row.warehouse_manager }}
               </q-td>
             </template>
             <q-td key="creater" :props="props">
               {{ props.row.creater }}
             </q-td>
             <q-td key="create_time" :props="props">
               {{ props.row.create_time }}
             </q-td>
             <q-td key="update_time" :props="props">
               {{ props.row.update_time }}
             </q-td>
             <template v-if="!editMode">
               <q-td key="action" :props="props" style="width: 100px">
                 <q-btn round flat push color="purple" icon="edit" @click="editData(props.row)">
                   <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                    {{ $t('edit') }}
                  </q-tooltip>
                 </q-btn>
                 <q-btn round flat push color="dark" icon="delete" @click="deleteData(props.row.id)">
                   <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                    {{ $t('delete') }}
                  </q-tooltip>
                 </q-btn>
               </q-td>
               </template>
             <template v-else-if="editMode">
               <template v-if="props.row.id === editid">
                 <q-td key="action" :props="props" style="width: 100px">
                 <q-btn round flat push color="secondary" icon="check" @click="editDataSubmit()">
                   <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                    {{ $t('confirmedit') }}
                  </q-tooltip>
                 </q-btn>
                 <q-btn round flat push color="red" icon="close" @click="editDataCancel()">
                   <q-tooltip content-class="bg-amber text-black shadow-4" :offset="[10, 10]" content-style="font-size: 12px">
                    {{ $t('canceledit') }}
                  </q-tooltip>
                 </q-btn>
               </q-td>
               </template>
                <template v-else-if="props.row.id !== editid"></template>
             </template>
           </q-tr>
         </template>
        </q-table>
      </transition>
      <template>
        <div v-show="max !== 0" class="q-pa-lg flex flex-center">
           <div>{{ total }} </div>
          <q-pagination
            v-model="current"
            color="black"
            :max="max"
            :max-pages="6"
            boundary-links
            @click="getList()"
          />
          <div>
            <input
              v-model="paginationIpt"
              @blur="changePageEnter"
              style="width: 60px; text-align: center"
            />
          </div>
        </div>
        <div v-show="max === 0" class="q-pa-lg flex flex-center">
          <q-btn flat push color="dark" :label="$t('no_data')"></q-btn>
        </div>
    </template>
      <q-dialog v-model="newForm">
       <q-card class="shadow-24">
         <q-bar class="bg-light-blue-10 text-white rounded-borders" style="height: 50px">
           <div>{{ $t('newtip') }}</div>
           <q-space />
           <q-btn dense flat icon="close" v-close-popup>
             <q-tooltip content-class="bg-amber text-black shadow-4">{{ $t('index.close') }}</q-tooltip>
           </q-btn>
         </q-bar>
         <q-card-section style="max-height: 325px; width: 400px" class="scroll">
           <q-input dense
                    outlined
                    square
                    v-model="newFormData.warehouse_name"
                    :label="$t('warehouse.view_warehouse.warehouse_name')"
                    autofocus
                    :rules="[ val => val && val.length > 0 || error1]"
                    @keyup.enter="newDataSubmit()"/>
           <q-input dense
                    outlined
                    square
                    v-model="newFormData.warehouse_city"
                    :label="$t('warehouse.view_warehouse.warehouse_city')"
                    :rules="[ val => val && val.length > 0 || error2]"
                    @keyup.enter="newDataSubmit()"/>
           <q-input dense
                    outlined
                    square
                    v-model="newFormData.warehouse_address"
                    :label="$t('warehouse.view_warehouse.warehouse_address')"
                    :rules="[ val => val && val.length > 0 || error3]"
                    @keyup.enter="newDataSubmit()"/>
           <q-input dense
                    outlined
                    square
                    v-model="newFormData.warehouse_contact"
                    :label="$t('warehouse.view_warehouse.warehouse_contact')"
                    :rules="[ val => val && val.length > 0 || error4]"
                    @keyup.enter="newDataSubmit()"/>
           <q-input dense
                    outlined
                    square
                    v-model="newFormData.warehouse_manager"
                    :label="$t('warehouse.view_warehouse.warehouse_manager')"
                    :rules="[ val => val && val.length > 0 || error5]"
                    @keyup.enter="newDataSubmit()"/>
         </q-card-section>
         <div style="float: right; padding: 15px 15px 15px 0">
           <q-btn color="white" text-color="black" style="margin-right: 25px" @click="newDataCancel()">{{ $t('cancel') }}</q-btn>
           <q-btn color="primary" @click="newDataSubmit()">{{ $t('submit') }}</q-btn>
         </div>
       </q-card>
     </q-dialog>
      <q-dialog v-model="deleteForm">
       <q-card class="shadow-24">
         <q-bar class="bg-light-blue-10 text-white rounded-borders" style="height: 50px">
           <div>{{ $t('delete') }}</div>
           <q-space />
           <q-btn dense flat icon="close" v-close-popup>
             <q-tooltip content-class="bg-amber text-black shadow-4">{{ $t('index.close') }}</q-tooltip>
           </q-btn>
         </q-bar>
         <q-card-section style="max-height: 325px; width: 400px" class="scroll">
           {{ $t('deletetip') }}
         </q-card-section>
         <div style="float: right; padding: 15px 15px 15px 0">
           <q-btn color="white" text-color="black" style="margin-right: 25px" @click="deleteDataCancel()">{{ $t('cancel') }}</q-btn>
           <q-btn color="primary" @click="deleteDataSubmit()">{{ $t('submit') }}</q-btn>
         </div>
       </q-card>
     </q-dialog>
    </div>
</template>
    <router-view />

<script>
import { getauth, postauth, putauth, deleteauth } from 'boot/axios_request'
import { LocalStorage } from 'quasar'

export default {
  name: 'Pagewarehouse',
  data () {
    return {
      openid: '',
      login_name: '',
      authin: '0',
      pathname: 'warehouse/',
      pathname_previous: '',
      pathname_next: '',
      separator: 'cell',
      loading: false,
      height: '',
      table_list: [],
      columns: [
        { name: 'warehouse_name', required: true, label: this.$t('warehouse.view_warehouse.warehouse_name'), align: 'left', field: 'warehouse_name' },
        { name: 'warehouse_city', label: this.$t('warehouse.view_warehouse.warehouse_city'), field: 'warehouse_city', align: 'center' },
        { name: 'warehouse_address', label: this.$t('warehouse.view_warehouse.warehouse_address'), field: 'warehouse_address', align: 'center' },
        { name: 'warehouse_contact', label: this.$t('warehouse.view_warehouse.warehouse_contact'), field: 'warehouse_contact', align: 'center' },
        { name: 'warehouse_manager', label: this.$t('warehouse.view_warehouse.warehouse_manager'), field: 'warehouse_manager', align: 'center' },
        { name: 'creater', label: this.$t('creater'), field: 'creater', align: 'center' },
        { name: 'create_time', label: this.$t('createtime'), field: 'create_time', align: 'center' },
        { name: 'update_time', label: this.$t('updatetime'), field: 'update_time', align: 'center' },
        { name: 'action', label: this.$t('action'), align: 'right' }
      ],
      filter: '',
      pagination: {
        page: 1,
        rowsPerPage: '30'
      },
      newForm: false,
      newFormData: {
        warehouse_name: '',
        warehouse_city: '',
        warehouse_address: '',
        warehouse_contact: '',
        warehouse_manager: '',
        creater: ''
      },
      editid: 0,
      editFormData: {},
      editMode: false,
      deleteForm: false,
      deleteid: 0,
      error1: this.$t('warehouse.view_warehouseset.error1'),
      error2: this.$t('warehouse.view_warehouseset.error2'),
      error3: this.$t('warehouse.view_warehouseset.error3'),
      error4: this.$t('warehouse.view_warehouseset.error4'),
      error5: this.$t('warehouse.view_warehouseset.error5'),
      current: 1,
      max: 0,
      total: 0,
      paginationIpt: 1
    }
  },
  methods: {
    getList () {
      var _this = this
      if (LocalStorage.has('auth')) {
        getauth(_this.pathname + '?page=' + '' + _this.current, {
        }).then(res => {
          _this.table_list = res.results
          _this.total = res.count
          if (res.count === 0) {
            _this.max = 0
          } else {
            if (Math.ceil(res.count / 30) === 1) {
              _this.max = 0
            } else {
              _this.max = Math.ceil(res.count / 30)
            }
          }
          _this.pathname_previous = res.previous
          _this.pathname_next = res.next
        }).catch(err => {
          _this.$q.notify({
            message: err.detail,
            icon: 'close',
            color: 'negative'
          })
        })
      }
    },
    changePageEnter(e) {
      if (Number(this.paginationIpt) < 1) {
        this.current = 1;
        this.paginationIpt = 1;
      } else if (Number(this.paginationIpt) > this.max) {
        this.current = this.max;
        this.paginationIpt = this.max;
      } else {
        this.current = Number(this.paginationIpt);
      }
      this.getList();
    },
    getListPrevious () {
      var _this = this
      if (LocalStorage.has('auth')) {
        getauth(_this.pathname_previous, {
        }).then(res => {
          _this.table_list = res.results
          _this.pathname_previous = res.previous
          _this.pathname_next = res.next
        }).catch(err => {
          _this.$q.notify({
            message: err.detail,
            icon: 'close',
            color: 'negative'
          })
        })
      }
    },
    getListNext () {
      var _this = this
      if (LocalStorage.has('auth')) {
        getauth(_this.pathname_next, {
        }).then(res => {
          _this.table_list = res.results
          _this.pathname_previous = res.previous
          _this.pathname_next = res.next
        }).catch(err => {
          _this.$q.notify({
            message: err.detail,
            icon: 'close',
            color: 'negative'
          })
        })
      } else {
      }
    },
    reFresh () {
      var _this = this
      _this.getList()
    },
    newFormCheck () {
      var _this = this
      if (_this.table_list.length >= 1) {
        _this.$q.notify({
          message: 'You Just Can Create 1 Line Data',
          icon: 'close',
          color: 'negative'
        })
      } else {
        _this.newForm = true
      }
    },
    newDataSubmit () {
      var _this = this
      var warehousesets = []
      _this.table_list.forEach(i => {
        warehousesets.push(i.warehouse_name)
      })
      if (warehousesets.indexOf(_this.newFormData.warehouse_name) === -1 && _this.newFormData.warehouse_name.length !== 0) {
        _this.newFormData.creater = _this.login_name
        postauth(_this.pathname, _this.newFormData).then(res => {
          _this.getList()
          _this.newDataCancel()
          _this.$q.notify({
            message: 'Success Create',
            icon: 'check',
            color: 'green'
          })
        }).catch(err => {
          _this.$q.notify({
            message: err.detail,
            icon: 'close',
            color: 'negative'
          })
        })
      } else {
        _this.newFormData.creater = _this.login_name
        postauth(_this.pathname, _this.newFormData).then(res => {
          _this.getList()
          _this.newDataCancel()
          _this.$q.notify({
            message: '',
            icon: 'close',
            color: 'negative'
          })
        }).catch(err => {
          _this.$q.notify({
            message: err.detail,
            icon: 'close',
            color: 'negative'
          })
        })
      }
      warehousesets = []
    },
    newDataCancel () {
      var _this = this
      _this.newForm = false
      _this.newFormData = {
        warehouse_name: '',
        warehouse_city: '',
        warehouse_address: '',
        warehouse_contact: '',
        warehouse_manager: '',
        creater: ''
      }
    },
    editData (e) {
      var _this = this
      _this.editMode = true
      _this.editid = e.id
      _this.editFormData = {
        warehouse_name: e.warehouse_name,
        warehouse_city: e.warehouse_city,
        warehouse_address: e.warehouse_address,
        warehouse_contact: e.warehouse_contact,
        warehouse_manager: e.warehouse_manager,
        creater: _this.login_name
      }
    },
    editDataSubmit () {
      var _this = this
      putauth(_this.pathname + _this.editid + '/', _this.editFormData).then(res => {
        _this.editDataCancel()
        _this.getList()
        _this.$q.notify({
          message: 'Success Edit Data',
          icon: 'check',
          color: 'green'
        })
      }).catch(err => {
        _this.$q.notify({
          message: err.detail,
          icon: 'close',
          color: 'negative'
        })
      })
    },
    editDataCancel () {
      var _this = this
      _this.editMode = false
      _this.editid = 0
      _this.editFormData = {
        warehouse_name: '',
        warehouse_city: '',
        warehouse_address: '',
        warehouse_contact: '',
        warehouse_manager: '',
        creater: ''
      }
    },
    deleteData (e) {
      var _this = this
      _this.deleteForm = true
      _this.deleteid = e
    },
    deleteDataSubmit () {
      var _this = this
      deleteauth(_this.pathname + _this.deleteid + '/').then(res => {
        _this.deleteDataCancel()
        _this.getList()
        _this.$q.notify({
          message: 'Success Edit Data',
          icon: 'check',
          color: 'green'
        })
      }).catch(err => {
        _this.$q.notify({
          message: err.detail,
          icon: 'close',
          color: 'negative'
        })
      })
    },
    deleteDataCancel () {
      var _this = this
      _this.deleteForm = false
      _this.deleteid = 0
    }
  },
  created () {
    var _this = this
    if (LocalStorage.has('openid')) {
      _this.openid = LocalStorage.getItem('openid')
    } else {
      _this.openid = ''
      LocalStorage.set('openid', '')
    }
    if (LocalStorage.has('login_name')) {
      _this.login_name = LocalStorage.getItem('login_name')
    } else {
      _this.login_name = ''
      LocalStorage.set('login_name', '')
    }
    if (LocalStorage.has('auth')) {
      _this.authin = '1'
      _this.getList()
    } else {
      _this.authin = '0'
    }
  },
  mounted () {
    var _this = this
    if (_this.$q.platform.is.electron) {
      _this.height = String(_this.$q.screen.height - 290) + 'px'
    } else {
      _this.height = _this.$q.screen.height - 290 + '' + 'px'
    }
  },
  updated () {
  },
  destroyed () {
  }
}
</script>
