<template>
  <div class="container text-left" >

    <div class="text-center">
      <h3>{{ pageTitle }}</h3>
    </div>

    <form>

      <div class="row">
        <div class="col-9">

          <b-form-group id="nameLabel" label="Seu nome:" label-for="name" >
            <b-form-input id="name" type="text" v-model="form.name"  placeholder="Nome completo"></b-form-input>
          </b-form-group>

          <b-form-group id="emailLabel" label="Email:" label-for="email">
            <b-form-input  id="email" type="email" v-model="form.email"  placeholder="Enter email"></b-form-input>
          </b-form-group>

          <b-form-group id="loginLabel" label="Login:" label-for="login" >
            <b-form-input id="login" type="text" v-model="form.login"  placeholder="An diferent and creative login"></b-form-input>
          </b-form-group>

        </div>
        
        <div class="col ">
          <b-form-group id="avatarLabel" label="Avatar:" label-for="role" class="" >
            <vue-dropzone
              ref="myVueDropzone"
              id="dropzone"
              :options="dropzoneOptions"
              v-on:vdropzone-sending="getExtraUploadData"
              v-on:vdropzone-success="setAvatarData"
              :useCustomSlot=true
             >
              <div class="dropzone-custom-content">
                <h3 class="dropzone-custom-title">Arraste uma imagem aqui!</h3>
                <div class="subtitle">...ou clique para selecionar uma</div>
              </div>
            </vue-dropzone>
          </b-form-group>
          <b-alert show variant="info" v-if="form._id">Deixe em branco para manter a mesma</b-alert>
        </div>

        

      </div>

      <div class="row">
       
      </div>

      <div class="row">
        <b-form-group class="col" id="passwordLabel" label="Senha:" label-for="password" >
          <b-form-input id="password" type="password" v-model="form.password"  placeholder="Uma senha"></b-form-input>
        </b-form-group>

        <b-form-group class="col" id="passwordCLabel" label="Confirmação de senha:" label-for="passwordC" >
          <b-form-input id="passwordC" type="password" v-model="form.passwordC" placeholder="Repita a senha"
          :state="(form.password == form.passwordC) && form.password != ''"  class="error"></b-form-input>
        </b-form-group>

      </div>

      <b-alert show variant="info" v-if="form._id">Deixe em branco para manter a mesma senha</b-alert>

      <div class="text-center">
        <b-button type="button" variant="primary" @click="onSubmit" >Confirmar</b-button>
        <router-link to="/users">
          <b-button type="reset" variant="danger"  >
            Cancelar
          </b-button>
        </router-link>
      </div>
    </form>
  </div>
</template>

<script>

import vue2Dropzone from 'vue2-dropzone';

export default {
  props:['id'],
  data() {
    return {
      clients: [],
      form:{
        email: 'teste@test.com',
        login: 'teste',
        name: 'teste',
        password: 'teste',
        avatar: {
          filename: '',
          mimetype: '',
          folder: '',
        },
      },
      pageTitle: 'Meus dados',
      dropzoneOptions: this.$http.getDropzoneConfig(
        {
          addRemoveLinks: true,
          capture: true,
          parallelUploads: 1,
          uploadMultiple: false,
          thumbnailWidth: 150,
          acceptedFiles: 'image/*',
          maxFilesize: 2,
          autoProcessQueue: true,
          maxFiles: 1,
          dictDefaultMessage: 'UPLOAD ME',
        },
      ),
    };
  },
  methods: {
    setAvatarData(file, response) {
      this.form.avatar = {};
      this.form.avatar.filename = response.file.filename;
      this.form.avatar.mimetype = response.file.mimetype;
      this.form.avatar.folder = response.file.destinationFolder;
    },
    getExtraUploadData(file, xhr, formData) {
      formData.append('folder', 'avatar'); // add field to upload form
    },
    async onSubmit() {
      if (this.form._id) {
        if (this.form.email && this.form.login) {
          const response = await this.$http.put('v1/produtores', this.form); // request with async await

          if (response.success) {
            this.$toasted.show('User edited with success', { icon: 'check', type: 'success' });
          } else {
            this.$toasted.show(response.err, { icon: 'times', type: 'error' });
          }
        } else {
          this.$toasted.show('Inform  email, role, client, login and password. Password and passord confirm must be equals', { icon: 'times', type: 'error' });
        }
      } else if (
        this.form.client &&
        this.form.email &&
        this.form.login &&
        (this.form.password && this.form.password === this.form.passwordC)
      ) {
        const response = await this.$http.post('v1/produtores', this.form); // request with async await

        if (response.success) {
          this.$toasted.show('Registrarion completed with success', { icon: 'check', type: 'success' });
        } else {
          this.$toasted.show(response.err, { icon: 'times', type: 'error' });
        }
      } else {
        this.$toasted.show('Inform  email, role, client, login and password. Password and passord confirm must be equals', { icon: 'times', type: 'error' });
      }
    },
  },
  async mounted() {
    if (this.id) {
      const response = await this.$http.get(`/v1/produtores/${this.id}`);
      if (response._id) {
        delete response.password;
        this.form = response;
      } else {
        this.$toasted.show('An error has ocurred', { icon: 'times', type: 'error' });
      }
    }
  },
  components: {
    vueDropzone: vue2Dropzone,
  },
};
</script>

<style>
.dropzone-custom-content {
  border-radius: 50%;
  width: 200px;
  height: 200px;
  background-color: #314b5f;
  text-align: center;
}

.vue-dropzone{
  border-radius: 50%;
  width: 200px;
  height: 200px;
  margin: 0px auto;
  padding: 0px;
}

.dropzone img{
  border-radius: 50%;
  width: 200px;
  max-height: 200px;
}

.dropzone .dz-message, .dropzone .dz-preview{
  width: 200px;
  margin: 0px;
  padding: 0px;
}


.dropzone-custom-title {
  padding-top: 30%;
  color: #00b782;
  font-size: 18px;
}

.subtitle {
  color: #fff;
}
</style>
