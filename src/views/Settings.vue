<template>
  <div class="p-sm-2">
    <div class="my-3 pb-2">
      <div class="d-flex justify-content-between align-items-center">
        <h1>settings</h1>
        <b-dropdown variant="link" toggle-class="text-decoration-none p-0" no-caret right>
          <template v-slot:button-content>
            <svg
              width="18"
              height="4"
              viewBox="0 0 18 4"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M2 4C3.10457 4 4 3.10457 4 2C4 0.89543 3.10457 0 2 0C0.89543 0 0 0.89543 0 2C0 3.10457 0.89543 4 2 4Z"
                fill="#C3C6D1"
              />
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M9 4C10.1046 4 11 3.10457 11 2C11 0.89543 10.1046 0 9 0C7.89543 0 7 0.89543 7 2C7 3.10457 7.89543 4 9 4Z"
                fill="#C3C6D1"
              />
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M16 4C17.1046 4 18 3.10457 18 2C18 0.89543 17.1046 0 16 0C14.8954 0 14 0.89543 14 2C14 3.10457 14.8954 4 16 4Z"
                fill="#C3C6D1"
              />
            </svg>
          </template>
          <b-dropdown-item href="#" v-b-modal.seed-modal>View secret words</b-dropdown-item>
        </b-dropdown>
      </div>
    </div>

    <b-modal id="seed-modal" centered hide-footer>
      <template v-slot:modal-header="{ close }">
        <div class="px-2 px-sm-3 pt-2 d-flex justify-content-between w-100">
          <h3>secret words</h3>
          <!-- Emulate built in modal header close button action -->
          <a href="#" class="align-self-center" v-on:click.stop.prevent="close">
            <svg
              width="18"
              height="18"
              viewBox="0 0 18 18"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                clip-rule="evenodd"
                d="M13.6003 4.44197C13.3562 4.19789 12.9605 4.19789 12.7164 4.44197L9.02116 8.1372L5.32596 4.442C5.08188 4.19792 4.68615 4.19792 4.44207 4.442C4.198 4.68607 4.198 5.0818 4.44207 5.32588L8.13728 9.02109L4.44185 12.7165C4.19777 12.9606 4.19777 13.3563 4.44185 13.6004C4.68592 13.8445 5.08165 13.8445 5.32573 13.6004L9.02116 9.90497L12.7166 13.6004C12.9607 13.8445 13.3564 13.8445 13.6005 13.6004C13.8446 13.3563 13.8446 12.9606 13.6005 12.7165L9.90505 9.02109L13.6003 5.32585C13.8444 5.08178 13.8444 4.68605 13.6003 4.44197Z"
                fill="#6c757d"
              />
            </svg>
          </a>
        </div>
      </template>
      <seed></seed>
    </b-modal>

    <b-row>
      <b-col col cols="12" md="6" xl="4">
        <card-widget
          header="Tor"
          :status="{ text: 'Running', variant: 'success', blink: false }"
          title="100%"
          sub-title="Traffic relayed through Tor"
          icon="icon-app-tor.svg"
        >
          <div class="pt-2">
            <div class="d-flex w-100 justify-content-between px-3 px-lg-4 mb-4">
              <div>
                <span class="d-block">Bitcoin</span>
                <small class="d-block" style="opacity: 0.4">Run Bitcoin Core on Tor</small>
              </div>
              <toggle-switch
                class="align-self-center"
                disabled
                tooltip="Sorry, Tor cannot be disabled for now"
              ></toggle-switch>
            </div>
            <div class="d-flex w-100 justify-content-between px-3 px-lg-4 mb-4">
              <div>
                <span class="d-block">Lightning Network</span>
                <small class="d-block" style="opacity: 0.4">Run Lightning on Tor</small>
              </div>
              <toggle-switch
                class="align-self-center"
                disabled
                tooltip="Sorry, Tor cannot be disabled for now"
              ></toggle-switch>
            </div>
            <div class="px-3 px-lg-4 mb-4">
              <div class="d-flex justify-content-between w-100 mb-3">
                <div>
                  <span class="d-block">Remote Access</span>
                  <small class="d-block" style="opacity: 0.4">Remotely access your Umbrel on Tor</small>
                </div>
                <toggle-switch
                  class="align-self-center"
                  disabled
                  tooltip="Sorry, Tor cannot be disabled for now"
                ></toggle-switch>
              </div>
              <input-copy class="w-100" size="sm" :value="onionAddress"></input-copy>
            </div>
            <div class="px-3 px-lg-4 py-2"></div>
          </div>
        </card-widget>
      </b-col>
      <b-col col cols="12" md="6" xl="4">
        <card-widget header="Change password" :loading="isChangingPassword">
          <div class="px-4 pb-2">
            <label class="sr-onlsy" for="input-withdrawal-amount">Current password</label>
            <input-password
              v-model="currentPassword"
              ref="password"
              inputGroupClass="neu-input-group"
              :inputClass="[
                isIncorrectPassword ? 'incorrect-password' : '',
                'form-control form-control-lg neu-input w-100'
              ]"
              :disabled="isChangingPassword"
            />
            <div class="py-2"></div>
            <label class="sr-onlsy" for="input-withdrawal-amount">New password</label>
            <input-password
              v-model="newPassword"
              ref="password"
              inputGroupClass="neu-input-group"
              inputClass="form-control form-control-lg neu-input w-100"
              :disabled="isChangingPassword"
            />
            <div class="py-2"></div>
            <label class="sr-onlsy" for="input-withdrawal-amount">Confirm new password</label>
            <input-password
              v-model="confirmNewPassword"
              ref="password"
              inputGroupClass="neu-input-group"
              inputClass="form-control form-control-lg neu-input w-100"
              :disabled="isChangingPassword"
            />
            <div class="py-2"></div>
            <b-alert variant="warning" show>
              <small>
                ⚠ Remember, there is no "Forgot Password" button. If you lose
                your password, you will have to recover your Umbrel using your 24
                secret words.
              </small>
            </b-alert>
          </div>
          <b-button
            class="w-100"
            variant="success"
            style="border-radius: 0; border-bottom-left-radius: 1rem; border-bottom-right-radius: 1rem; padding-top: 1rem; padding-bottom: 1rem;"
            :disabled="isChangingPassword || !isAllowedToChangePassword"
            @click="changePassword"
          >{{ isChangingPassword ? 'Changing password...' : 'Change password'}}</b-button>
        </card-widget>
      </b-col>
      <b-col col cols="12" md="6" xl="4">
        <card-widget header="System" :loading="isCheckingForUpdate || isUpdating">
          <div class="pt-2">
            <div class="d-flex w-100 justify-content-between px-3 px-lg-4 mb-4">
              <div>
                <span class="d-block">Shutdown</span>
                <small class="d-block text-muted">Power off your Umbrel</small>
              </div>
              <b-button variant="outline-danger" size="sm" @click="shutdownPrompt">Shutdown</b-button>
            </div>
          </div>
          <div class="pt-2">
            <div class="d-flex w-100 justify-content-between px-3 px-lg-4 mb-4">
              <div>
                <span class="d-block">Reboot</span>
                <small class="d-block text-muted">Reboot your Umbrel</small>
              </div>

              <b-button variant="outline-danger" size="sm" @click="rebootPrompt">Reboot</b-button>
              <b-modal
                ref="reboot-modal"
                title="Are you sure?"
                no-close-on-backdrop
                no-close-on-esc
                @ok="reboot($event)"
              >
                <div>
                  <p>Don't forget to login to your dashboard after the restart is complete (required only once after a restart for your Umbrel to be online).</p>
                </div>
              </b-modal>
            </div>
          </div>
          <div class="pt-2">
            <div class="d-flex w-100 justify-content-between px-3 px-lg-4 mb-4">
              <div>
                <span class="d-block">Logs</span>
                <small class="d-block text-muted">View system logs in real-time</small>
              </div>
              <a class="card-link mr-2" href="/logs" target="_blank">
                <small class="text-uppercase">View</small>
              </a>
            </div>
          </div>
          <div class="px-4 pb-4">
            <div class="w-100 d-flex justify-content-between mb-2">
              <span class="align-self-end">Umbrel Version</span>
              <span class="font-weight-normal mb-0">{{ version }}</span>
            </div>
            <div v-show="!isCheckingForUpdate">
              <span v-show="!availableUpdate.version">
                <b-icon icon="check-circle-fill" variant="success"></b-icon>
                <small class="text-muted ml-1">Your Umbrel is on the latest version</small>
              </span>
              <div v-show="availableUpdate.version">
                <span class="d-block">
                  <b-icon icon="bell-fill" variant="success"></b-icon>
                  <small
                    class="text-muted ml-1"
                  >Umbrel v{{availableUpdate.version}} is now available to install</small>
                </span>
                <b-button
                  class="mt-2"
                  variant="primary"
                  size="sm"
                  @click.prevent="confirmUpdate"
                  :disabled="isUpdating"
                >Install now</b-button>
              </div>
            </div>
          </div>
          <b-button
            class="w-100"
            variant="success"
            style="border-radius: 0; border-bottom-left-radius: 1rem; border-bottom-right-radius: 1rem; padding-top: 1rem; padding-bottom: 1rem;"
            :disabled="isCheckingForUpdate || isUpdating"
            @click="checkForUpdate"
          >
            <b-icon icon="arrow-repeat" class="mr-2" :animation="isCheckingForUpdate ? 'spin' : ''"></b-icon>
            {{ isCheckingForUpdate ? "Checking for update" : "Check for update"}}
          </b-button>
        </card-widget>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import { mapState } from "vuex";
import API from "@/helpers/api";

import CardWidget from "@/components/CardWidget";
import ToggleSwitch from "@/components/ToggleSwitch";
import Seed from "@/components/Seed";
import InputPassword from "@/components/Utility/InputPassword";
import InputCopy from "@/components/Utility/InputCopy";

export default {
  data() {
    return {
      currentPassword: "",
      isIncorrectPassword: false,
      newPassword: "",
      confirmNewPassword: "",
      isChangingPassword: false,
      isCheckingForUpdate: false,
      isUpdating: false
    };
  },
  computed: {
    ...mapState({
      version: state => state.system.version,
      onionAddress: state => state.system.onionAddress,
      availableUpdate: state => state.system.availableUpdate,
      updateStatus: state => state.system.updateStatus
    }),
    isAllowedToChangePassword() {
      if (!this.currentPassword) {
        return false;
      }
      if (this.newPassword.length < 12) {
        return false;
      }
      if (this.newPassword !== this.confirmNewPassword) {
        return false;
      }
      if (this.currentPassword === this.newPassword) {
        return false;
      }
      return true;
    }
  },
  created() {
    this.$store.dispatch("system/getOnionAddress");
    this.$store.dispatch("system/getVersion");
  },
  methods: {
    async changePassword() {
      // disable on testnet
      if (window.location.hostname === "testnet.getumbrel.com") {
        return this.$bvToast.toast('y u try to do dis on testnet :"(', {
          title: "Error",
          autoHideDelay: 3000,
          variant: "danger",
          solid: true,
          toaster: "b-toaster-bottom-right"
        });
      }

      const payload = {
        password: this.currentPassword,
        newPassword: this.newPassword
      };

      this.isChangingPassword = true;

      try {
        await API.post(
          `${process.env.VUE_APP_MANAGER_API_URL}/v1/account/change-password`,
          payload,
          false
        );
      } catch (error) {
        if (error.response && error.response.data) {
          this.$bvToast.toast(error.response.data, {
            title: "Error",
            autoHideDelay: 3000,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-right"
          });
          if (error.response.data === "Incorrect password") {
            this.isIncorrectPassword = true;
          }
        }
        this.isChangingPassword = false;
        return;
      }

      this.$bvToast.toast(
        `You've successfully changed your Umbrel's password`,
        {
          title: "Password Changed",
          autoHideDelay: 3000,
          variant: "success",
          solid: true,
          toaster: "b-toaster-bottom-right"
        }
      );

      this.isChangingPassword = false;

      // Remove passwords from view
      this.currentPassword = "";
      this.newPassword = "";
      this.confirmNewPassword = "";
    },
    confirmUpdate() {
      this.$store.dispatch("system/confirmUpdate");
    },
    async checkForUpdate() {
      this.isCheckingForUpdate = true;
      await this.$store.dispatch("system/getAvailableUpdate");
      this.isCheckingForUpdate = false;
    },
    async shutdownPrompt() {
      // disable on testnet
      if (window.location.hostname === "testnet.getumbrel.com") {
        return this.$bvToast.toast('y u try to do dis on testnet :"(', {
          title: "Error",
          autoHideDelay: 3000,
          variant: "danger",
          solid: true,
          toaster: "b-toaster-bottom-right"
        });
      }

      // Get user consent first
      const approved = await this.$bvModal.msgBoxConfirm(
        "Your lightning wallet will not be able to receive any payments while your Umbrel is offline. Also, don't forget to login to your dashboard after you restart (required only once after a restart for your Umbrel to be online).",
        { title: "Are you sure?" }
      );
      if (!approved) {
        return;
      }

      // Shutdown request
      let toastText = "";
      let toastOptions = {
        autoHideDelay: 3000,
        solid: true,
        toaster: "b-toaster-bottom-right"
      };
      try {
        await this.$store.dispatch("system/shutdown");
      } catch (e) {
        toastText = "Shutdown failed";
        toastOptions.title =
          "Something went wrong and Umbrel was not able to shutdown";
        toastOptions.variant = "danger";
      }
      this.$bvToast.toast(toastText, toastOptions);
    },
    rebootPrompt() {
      // disable on testnet
      if (window.location.hostname === "testnet.getumbrel.com") {
        return this.$bvToast.toast('y u try to do dis on testnet :"(', {
          title: "Error",
          autoHideDelay: 3000,
          variant: "danger",
          solid: true,
          toaster: "b-toaster-bottom-right"
        });
      }
      // Reset any cached hasRebooted value from previous reboot
      this.$store.commit("system/setHasRebooted", false);
      this.$refs["reboot-modal"].show();
    },
    async reboot(event) {
      if (!this.hasRebooted) {
        event.preventDefault();
        try {
          await this.$store.dispatch("system/reboot");
        } catch (e) {
          this.$bvToast.toast("Reboot failed", {
            title: "Something went wrong and Umbrel was not able to reboot",
            autoHideDelay: 3000,
            variant: "danger",
            solid: true,
            toaster: "b-toaster-bottom-right"
          });
        }
      }
    }
  },
  beforeDestroy() {
    if (this.pollUpdateStatus) {
      window.clearInterval(this.pollUpdateStatus);
    }
  },
  watch: {
    currentPassword: function() {
      this.isIncorrectPassword = false;
    }
  },
  components: {
    CardWidget,
    ToggleSwitch,
    InputPassword,
    InputCopy,
    Seed
  }
};
</script>

<style lang="scss" scoped></style>
