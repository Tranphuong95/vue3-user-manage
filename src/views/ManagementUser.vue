<template>
  <h1 style="font-size: 18px; font-weight: bold">Quản lý người dùng</h1>
  <div class="btn-group">
    <a-button type="primary" @click="add">Thêm mới</a-button>
  </div>
  <a-table :dataSource="dataSource" :columns="columns">
    <template #bodyCell="{ column, record }">
      <template v-if="column.key === 'action'">
        <EditOutlined
          style="cursor: pointer; color: #40a9ff"
          @click="editData(record)"
        />
        <EyeOutlined
          style="cursor: pointer; margin-left: 12px; color: green"
          @click="viewData(record)"
        />
        <DeleteOutlined
          style="cursor: pointer; margin-left: 12px; color: red"
          @click="deleteData(record)"
        />
      </template>
    </template>
  </a-table>
  <a-modal
    v-model:visible="visible"
    title="Thêm mới người dùng"
    :footer="null"
    :closable="false"
    :maskClosable="false"
  >
    <a-form :label-col="labelCol" :wrapper-col="wrapperCol">
      <a-form-item label="Name" required>
        <a-input v-model:value="modelRef.name" :readOnly="disabledForm" />
      </a-form-item>
      <a-form-item label="Age" required>
        <a-input v-model:value="modelRef.age" :readOnly="disabledForm" />
      </a-form-item>
      <a-form-item label="Address" required>
        <a-input v-model:value="modelRef.address" :readOnly="disabledForm" />
      </a-form-item>
    </a-form>
    <div style="display: flex; justify-content: center">
      <a-button @click="closeModal">Đóng</a-button>
      <template v-if="!disabledForm">
        <a-button
          @click.prevent="submitForm"
          style="margin-left: 16px"
          type="primary"
          >Lưu</a-button
        >
      </template>
    </div>
  </a-modal>
</template>
<script lang="ts">
import { reactive, defineComponent, ref, toRaw, createVNode } from "vue";
import { Form, Modal } from "ant-design-vue";
import { v4 as uuidv4 } from "uuid";
import {
  EyeOutlined,
  EditOutlined,
  DeleteOutlined,
  ExclamationCircleOutlined,
} from "@ant-design/icons-vue";

const useForm = Form.useForm;

interface DataItem {
  key: string;
  name: string;
  age: number;
  address: string;
  id: String;
}

export default defineComponent({
  setup() {
    const visible = ref<boolean>(false);
    const disabledForm = ref<boolean>(false);
    const modelRef = ref<DataItem>({
      key: "",
      name: "",
      age: 0,
      address: "",
      id: "",
    });

    const rulesRef = reactive({
      name: [
        {
          required: true,
          message: "Please input name",
        },
      ],
      age: [
        {
          required: true,
          message: "Please input number age",
        },
      ],
      address: [
        {
          required: true,
          message: "Please input address",
        },
      ],
    });
    const { resetFields, validate, validateInfos, mergeValidateInfo } = useForm(
      modelRef,
      rulesRef
    );
    const dataSource = ref<Array<DataItem>>([
        {
          id: uuidv4(),
          key: "1",
          name: "Mike",
          age: 32,
          address: "10 Downing Street",
        },
        {
          id: uuidv4(),
          key: "2",
          name: "John",
          age: 42,
          address: "10 Downing Street",
        },
      ]),
      columns = [
        {
          title: "Name",
          dataIndex: "name",
          key: "name",
        },
        {
          title: "Age",
          dataIndex: "age",
          key: "age",
        },
        {
          title: "Address",
          dataIndex: "address",
          key: "address",
        },
        {
          title: "Action",
          dataIndex: "action",
          key: "action",
        },
      ];
    const showModal = () => {
      console.log("show form", this);
      visible.value = true;
    };
    const closeModal = () => {
      visible.value = false;
    };
    const confirm = (data: String, ok: (a: String) => void) => {
      Modal.confirm({
        title: "Xác nhận xóa người dùng",
        icon: createVNode(ExclamationCircleOutlined),
        content: `Bạn có chắc muốn xóa người dùng: ${data}`,
        okText: "Đồng ý",
        cancelText: "Hủy",
        onOk: ok,
      });
    };
    const add = () => {
      disabledForm.value = false;
      modelRef.value = { key: "", name: "", age: 0, address: "", id: "" };
      showModal();
    };
    const editData = (data: DataItem) => {
      disabledForm.value = false;
      modelRef.value = { ...data };
      showModal();
    };
    const handleDelete = (id: String) => {
      dataSource.value = dataSource.value.filter((f) => f.id !== id);
    };
    const deleteData = (data: DataItem) => {
      const { id } = data;
      if (!id) return;
      confirm(data.name, () => handleDelete(id));
    };
    const viewData = (data: DataItem) => {
      modelRef.value = { ...data };
      disabledForm.value = true;
      showModal();
    };
    const submitForm = () => {
      validate()
        .then(() => {
          const newData = toRaw(modelRef.value);
          const dataForm: DataItem = { ...newData, key: uuidv4() };
          console.log("dataForm", dataForm);
          const id = dataForm?.id;
          if (!newData.id) {
            dataSource.value.push({ ...dataForm, id: uuidv4() });
          } else {
            const index = dataSource.value?.findIndex((f) => f.id === id);
            if (index >= 0) {
              dataSource.value[index] = dataForm;
            }
          }
          resetFields();
          closeModal();
        })
        .catch((err: any) => {
          console.log("error", err);
        });
    };
    return {
      visible,
      modelRef,
      dataSource,
      columns,
      showModal,
      closeModal,
      add,
      editData,
      viewData,
      submitForm,
      deleteData,
      disabledForm,
      labelCol: { span: 4 },
      wrapperCol: { span: 14 },
    };
  },
  components: {
    EditOutlined,
    EyeOutlined,
    DeleteOutlined,
  },
});
</script>
<style lang="css">
.btn-group {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin: 0 0 12px 0;
}
</style>
