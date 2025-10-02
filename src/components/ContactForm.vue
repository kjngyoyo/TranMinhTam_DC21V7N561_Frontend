<template>
  <Form @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>
    <div class="form-group">
        <label>Nghề nghiệp</label>
        <Field type = "radio" name="job" value="Sinh viên"/> Sinh viên
        <Field type = "radio" name="job" value="Giáo viên"/> Giáo viên
        <Field type = "radio" name="job" value="Giao hàng"/> Giao hàng
        <Field type = "radio" name="job" value="Tài xế"/> Tài xế
          <Field type="radio" name="job" value="Khác" class="ml-3" v-model="selectedJob" /> Khác
          <div v-if="selectedJob === 'Khác'" class="mt-2">
            <Field name="job_other" type="text"placeholder="Nhập nghề nghiệp khác..." class="form-control" />
          </div>
          <ErrorMessage name="job" class="error-feedback" />
    </div>
    <div class="form-group">
        <label>Giới tính</label>
        <Field type = "radio" name="gender" value="Nam"/> Nam
        <Field type = "radio" name="gender" value="Nữ"/> Nữ
        <Field type = "radio" name="gender" value="Khác"/> Khác
        <br />
          <ErrorMessage name="job" class="error-feedback" />
    </div>
    <div class="form-group form-check">
      <Field name="favorite" type="checkbox" class="form-check-input" />
      <label for="favorite" class="form-check-label">
        <strong>Liên hệ yêu thích</strong>
      </label>
    </div>
    <div class="form-group">
      <button class="btn btn-primary">Lưu</button>
      <button v-if="contact._id" type="button" class="ml-2 btn btn-danger" @click="deleteContact">
        Xóa
      </button>
      <button type="button" class="ml-2 btn btn-danger" @click="Cancel">
        Thoát
      </button>
    </div>
  </Form>
</template>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
export default {
  components: { Form, Field, ErrorMessage },
  props: {
    contact: { type: Object, required: true }
  },
  emits: ["submit:contact", "delete:contact"],
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup.string().required("Tên phải có giá trị.").min(2).max(50),
      email: yup.string().email("E-mail không đúng.").max(50),
      address: yup.string().max(100),
      phone: yup.string().matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, "Số điện thoại không hợp lệ."),
    });
    return { contactFormSchema };
  },
  methods: {
    submitContact(values) {
      this.$emit("submit:contact", { ...this.contact, ...values });
    },
    deleteContact() {
      this.$emit("delete:contact", this.contact._id);
    },
    Cancel() {
      if (window.confirm("Bạn có chắc muốn thoát mà không lưu?")) {
        this.$router.push({ name: "contactbook" });
      }
    }
  }
};
</script>
<style scoped>
@import "@/assets/form.css";
</style>
