<template>
  <div class="login">
    <card title="Login">
        <form @submit.prevent="handleSubmit">
            <div>
                <fg-input label="Username" type="text" v-model="username" class="hr"/>
                <div v-show="submitted && !username" class="invalid-feedback">Username is required</div>
            </div>

            <div>
                <fg-input label="Password" type="password" v-model="password" class="hr"/>
                <div v-show="submitted && !password" class="invalid-feedback">Password is required</div>
            </div>

            <div>
                <button class="btn btn-primary fullWidth" :disabled="loggingIn">Login</button>
            </div>
        </form>
    </card>
  </div>
</template>

<script>
import FgInput from '../components/Inputs/formGroupInput';
import Card from '../components/Cards/Card';

export default {
    components: {
      FgInput,
      Card
    },
    data () {
        return {
            username: '',
            password: '',
            submitted: false
        }
    },
    computed: {
        loggingIn () {
            return this.$store.state.authentication.status.loggingIn;
        }
    },
    created () {
        // reset login status
        this.$store.dispatch('authentication/logout');
    },
    methods: {
        handleSubmit (e) {
            this.submitted = true;
            const { username, password } = this;
            const { dispatch } = this.$store;
            if (username && password) {
                dispatch('authentication/login', { username, password });
            }
        }
    }
};
</script>

<style scoped>
.login {
  display: flex;
  flex-direction: column;
  max-width: 800;
  align-items: center;
  justify-content: center;
  margin-top: 10%;
}
.fullWidth {
  width: 100%;
}
.hr {
  border-bottom: 1px solid lightgray;
}
</style>
