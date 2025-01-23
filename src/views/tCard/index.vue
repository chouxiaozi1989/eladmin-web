<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">id</label>
        <el-input v-model="query.cardId" clearable placeholder="id" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">用户id</label>
        <el-input v-model="query.userId" clearable placeholder="用户id" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">姓名</label>
        <el-input v-model="query.realName" clearable placeholder="姓名" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">手机号码</label>
        <el-input v-model="query.mobile" clearable placeholder="手机号码" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">办公电话</label>
        <el-input v-model="query.phoneNumber" clearable placeholder="办公电话" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">电子信箱</label>
        <el-input v-model="query.email" clearable placeholder="电子信箱" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">通讯地址</label>
        <el-input v-model="query.address" clearable placeholder="通讯地址" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">邮编</label>
        <el-input v-model="query.postcode" clearable placeholder="邮编" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">单位</label>
        <el-input v-model="query.organization" clearable placeholder="单位" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">部门</label>
        <el-input v-model="query.department" clearable placeholder="部门" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">职务</label>
        <el-input v-model="query.position" clearable placeholder="职务" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">有效标志</label>
        <el-input v-model="query.valid" clearable placeholder="有效标志" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">名片背景图</label>
        <el-input v-model="query.background" clearable placeholder="名片背景图" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="姓名" prop="realName">
            <el-input v-model="form.realName" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="手机号码" prop="mobile">
            <el-input v-model="form.mobile" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="办公电话" prop="phoneNumber">
            <el-input v-model="form.phoneNumber" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="电子信箱" prop="email">
            <el-input v-model="form.email" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="通讯地址" prop="address">
            <el-input v-model="form.address" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="邮编" prop="postcode">
            <el-input v-model="form.postcode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="单位" prop="organization">
            <el-input v-model="form.organization" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="部门" prop="department">
            <el-input v-model="form.department" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="职务" prop="position">
            <el-input v-model="form.position" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="名片背景图">
            <el-input v-model="form.background" :rows="3" type="textarea" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.status.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="cardId" label="id" />
        <el-table-column prop="userId" label="用户id" />
        <el-table-column prop="realName" label="姓名" />
        <el-table-column prop="mobile" label="手机号码" />
        <el-table-column prop="phoneNumber" label="办公电话" />
        <el-table-column prop="email" label="电子信箱" />
        <el-table-column prop="address" label="通讯地址" />
        <el-table-column prop="postcode" label="邮编" />
        <el-table-column prop="organization" label="单位" />
        <el-table-column prop="department" label="部门" />
        <el-table-column prop="position" label="职务" />
        <el-table-column prop="valid" label="有效标志" />
        <el-table-column prop="background" label="名片背景图" />
        <el-table-column v-if="checkPer(['admin','tCard:edit','tCard:del'])" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudTCard from '@/api/tCard'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { cardId: null, userId: null, realName: null, mobile: null, phoneNumber: null, email: null, address: null, postcode: null, organization: null, department: null, position: null, valid: null, background: null }
export default {
  name: 'TCard',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: 'SmartCard', url: 'api/tCard', idField: 'cardId', sort: 'cardId,desc', crudMethod: { ...crudTCard }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'tCard:add'],
        edit: ['admin', 'tCard:edit'],
        del: ['admin', 'tCard:del']
      },
      rules: {
        realName: [
          { required: true, message: '姓名不能为空', trigger: 'blur' }
        ],
        mobile: [
          { required: true, message: '手机号码不能为空', trigger: 'blur' }
        ],
        phoneNumber: [
          { required: true, message: '办公电话不能为空', trigger: 'blur' }
        ],
        email: [
          { required: true, message: '电子信箱不能为空', trigger: 'blur' }
        ],
        address: [
          { required: true, message: '通讯地址不能为空', trigger: 'blur' }
        ],
        postcode: [
          { required: true, message: '邮编不能为空', trigger: 'blur' }
        ],
        organization: [
          { required: true, message: '单位不能为空', trigger: 'blur' }
        ],
        department: [
          { required: true, message: '部门不能为空', trigger: 'blur' }
        ],
        position: [
          { required: true, message: '职务不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'cardId', display_name: 'id' },
        { key: 'userId', display_name: '用户id' },
        { key: 'realName', display_name: '姓名' },
        { key: 'mobile', display_name: '手机号码' },
        { key: 'phoneNumber', display_name: '办公电话' },
        { key: 'email', display_name: '电子信箱' },
        { key: 'address', display_name: '通讯地址' },
        { key: 'postcode', display_name: '邮编' },
        { key: 'organization', display_name: '单位' },
        { key: 'department', display_name: '部门' },
        { key: 'position', display_name: '职务' },
        { key: 'valid', display_name: '有效标志' },
        { key: 'background', display_name: '名片背景图' }
      ]
    }
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
