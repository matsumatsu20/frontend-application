<template>
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper" @click.self="closeModal">
        <div class="modal-container">
          <div class="modal-header">
            <div class="modal-header-content">
              <slot name="modal-header-content" />
            </div>
            <span class="modal-header-default-button" @click="closeModal">
              ✕
            </span>
          </div>
          <div class="modal-body">
            <slot name="modal-title">
              <h2 class="title">
                {{title}}
              </h2>
            </slot>
          </div>
          <div class="modal-content">
            <slot name="modal-content" />
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'

export default {
  props: {
    title: {
      type: String,
      required: false
    }
  },
  computed: {
    ...mapGetters('user', [
      'showSignUpModal',
      'showSignUpAuthFlowModal',
      'showLoginModal',
      'showReportModal',
      'showProfileSettingsModal',
      'showRestrictEditArticleModal'
    ])
  },
  methods: {
    closeModal() {
      if (this.showSignUpModal) {
        this.setSignUpModal({ showSignUpModal: false })
      }
      if (this.showSignUpAuthFlowModal) {
        this.setSignUpAuthFlowModal({ showSignUpAuthFlowModal: false })
      }
      if (this.showLoginModal) {
        this.setLoginModal({ showLoginModal: false })
      }
      if (this.showReportModal) {
        this.setReportModal({ showReportModal: false })
      }
      if (this.showProfileSettingsModal) {
        this.setProfileSettingsModal({ showProfileSettingsModal: false })
      }
      if (this.showRestrictEditArticleModal) {
        this.setRestrictEditArticleModal({ showRestrictEditArticleModal: false })
        this.$router.push('/me/articles/public')
      }
      this.$emit('close')
      document.body.scrollTop = 0
      document.querySelector('html,body').style.overflow = ''
    },
    ...mapActions('user', [
      'setSignUpModal',
      'setSignUpAuthFlowModal',
      'setLoginModal',
      'setReportModal',
      'setProfileSettingsModal',
      'setRestrictEditArticleModal'
    ])
  }
}
</script>

<style lang="scss" scoped>
.modal {
  &-mask {
    background-color: rgba(0, 0, 0, 0.8);
    bottom: 0;
    display: table;
    height: 100vh;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    transition: opacity 0.3s ease;
    width: 100%;
    z-index: 9998;
  }

  &-wrapper {
    display: table-cell;
    vertical-align: middle;
  }

  &-container {
    background: #fff;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    margin: 10px auto;
    max-width: 800px;
    padding: 20px 30px;
    transition: all 0.3s ease;
    width: 80%;
  }

  &-body {
    .title {
      color: #030303;
      font-family: Times;
      font-size: 20px;
      font-weight: bold;
      letter-spacing: 0.2em;
      line-height: 24px;
      margin: 50px 20px 0;
      text-align: center;
    }
  }

  &-header {
    &-content {
      margin: -20px -30px 20px;
    }

    &-default-button {
      color: #6e6e6e;
      cursor: pointer;
      float: right;
      font-size: 26px;
    }
  }

  &-enter {
    opacity: 0;
  }

  &-leave-active {
    opacity: 0;
  }

  &-enter &-container,
  &-leave-active &-container {
    transform: scale(1.1);
  }
}

@media screen and (max-width: 550px) {
  .modal {
    &-wrapper {
      width: 100%;
      padding-bottom: 4000px;
    }

    &-container {
      height: 100%;
      margin-top: 0;
      max-width: 550px;
      width: calc(100% - 60px);
    }
  }

  .cover-screen {
    .modal-container {
      height: 100vh;
    }
  }
}
</style>
