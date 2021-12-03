<template>
  <form class="auth" @submit.prevent="authenticate" @reset.prevent="cancel">
    <div v-if="error" class="alert alert-danger">
      {{ error }}
    </div>
    <template v-if="credentials && credentials.length">
      <div
        v-for="cred in credentials"
        :key="cred.name"
        class="form-group"
      >
        <label :for="`${cred.name}Input`">{{ cred.title }}</label>
        <input
          :id="`${cred.name}Input`"
          :aria-describedby="`${cred.name}Help`"
          type="text"
          class="form-control"
        >
        <small
          :id="`${cred.name}Help`"
          class="form-text text-muted"
        >{{ cred.description }}</small>
      </div>
    </template>
    <div v-if="!error" class="form-group mt-4">
      <button class="btn btn-primary mr-2" type="submit">Authenticate</button>
      <button class="btn btn-link" type="reset">Cancel</button>
    </div>
  </form>
</template>

<script>
function b64encode(s) {
  return btoa(unescape(encodeURIComponent(s)))
}

function b64decode(s) {
  return decodeURIComponent(escape(atob(s)))
}

function getUTCtimestamp() {
  const date = new Date()
  return Math.floor(
    new Date(
      date.getUTCFullYear(),
      date.getUTCMonth(),
      date.getUTCDate(),
      date.getUTCHours(),
      date.getUTCMinutes(),
      date.getUTCSeconds()
    ).getTime() / 1000)
}

export default {
  name: 'Auth',
  data() {
    return {
      credentials: null,
      redirectUri: null,
      state: null,
      error: null
    }
  },
  methods: {
    authenticate(e) {
      console.log(e)
      const codeData = {
        expiresAt: getUTCtimestamp() + 60,
        type: 'code'
      }
      this.redirectUri.searchParams.set('code', b64encode(JSON.stringify(codeData)))
      this.redirectUri.searchParams.set('state', this.state)
      window.location.href = this.redirectUri.toString()
    },
    cancel(e) {
      this.redirectUri.searchParams.set('error', 'access_denied')
      this.redirectUri.searchParams.set('error_description', 'Cancelled by user')
      this.redirectUri.searchParams.set('state', this.state)
      window.location.href = this.redirectUri.toString()
    }
  },
  mounted() {
    const searchParams = new URLSearchParams(window.location.search)
    try {
      this.redirectUri = new URL(searchParams.get('redirect_uri'))
    } catch (e) {
      console.error(e)
      this.error = 'Unable to interpret GET parameter redirect_uri'
      return
    }
    this.state = searchParams.get('state') || ''
    let loginHint = null
    try {
      loginHint = JSON.parse(b64decode(searchParams.get('login_hint')))
    } catch (e) {
      console.error(e)
      this.error = 'Unable to interpret GET parameter login_hint'
      return
    }
    this.credentials = loginHint.credentials
  }
}
</script>

<style lang="scss" scoped>
.auth {
  width: 50%;
  min-width: 200px;
  max-width: 400px;
  margin: 0 auto 0 auto;
}
</style>
