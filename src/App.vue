<template>
  <div id="app" :style="colorMode">
    <div class="login-container">
      <el-form
        ref="loginForm"
        :model="loginForm"
        :rules="loginRules"
        class="login-form"
        autocomplete="on"
        label-position="left"
        v-loading="loading"
      >
        <div class="title-container">
          <h3 class="title">登录到代码分析平台</h3>
        </div>

        <el-form-item prop="username">
          <span class="svg-container">
            <svg-icon icon-class="user" />
          </span>
          <el-input
            ref="username"
            v-model="loginForm.username"
            id="username"
            placeholder="用户名/学号"
            name="username"
            type="text"
            tabindex="1"
            autocomplete="on"
          />
        </el-form-item>

        <el-tooltip v-model="capsTooltip" content="大写锁定已打开" placement="right" manual>
          <el-form-item prop="password">
            <span class="svg-container">
              <svg-icon icon-class="password" />
            </span>
            <el-input
              :key="passwordType"
              ref="password"
              v-model="loginForm.password"
              :type="passwordType"
              placeholder="密码"
              name="password"
              id="password"
              tabindex="2"
              autocomplete="on"
              @keyup.native="checkCapslock"
              @blur="capsTooltip = false"
              @keyup.enter.native="handleLogin"
            />
            <span class="show-pwd" @click="showPwd" :style="'display:'+isMSEdge">
              <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
            </span>
          </el-form-item>
        </el-tooltip>

        <el-button
          :loading="loading"
          type="primary"
          style="width:100%;margin-top:30px;"
          @click.native.prevent="handleLogin"
        >登录</el-button>
      </el-form>

      <el-dialog title="Or connect with" :visible.sync="showDialog">
        Can not be simulated on local, so please combine you own business simulation! ! !
        <br />
        <br />
        <br />
        <social-sign />
      </el-dialog>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<script>
import { validUsername } from "@/utils/validate";
import Vue from "vue";
import API from "@/api/api.js";

Vue.prototype.API = API;

export default {
  name: "Login",
  data() {
    const validateUsername = (rule, value, callback) => {
      if (value === "" || value === null) {
        callback(new Error("用户名/学号不能为空"));
      } else if (
        !isNaN(parseInt(value.substring(0, 1))) &&
        parseInt(value).toString() !== value
      ) {
        callback(new Error("用户名不能以数字开头"));
      } else if (value.length != 10 && parseInt(value).toString() === value) {
        callback(new Error("学号有误！"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (value.length < 8 || value.length > 20) {
        callback(new Error("请输入8~20位的账户密码"));
      } else {
        callback();
      }
    };
    return {
      loginForm: {
        username: "",
        password: "",
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", validator: validateUsername },
        ],
        password: [
          { required: true, trigger: "blur", validator: validatePassword },
        ],
      },
      passwordType: "password",
      capsTooltip: false,
      loading: false,
      showDialog: false,
      redirect: undefined,
      otherQuery: {},
      colorMode: {
        "--highlightBg": "#E5E5E5",
        "--cursor": "#333",
        "--bg": "#fff",
        "--svg": "#888",
      },
      colorModeName: "light",
      isMSEdge: "none",
      loading: false,
    };
  },
  /*watch: {
    $route: {
      handler: function (route) {
        const query = route.query;
        if (query) {
          this.redirect = query.redirect;
          this.otherQuery = this.getOtherQuery(query);
        }
      },
      immediate: true,
    },
  },*/
  created() {
    if (location.search === "?night=1") {
      this.colorMode = {
        "--highlightBg": "#1D1D1D",
        "--cursor": "#ccc",
        "--bg": "#202020",
        "--svg": "#aaa",
      };
      this.colorModeName = "dark";
    } else {
      this.colorMode = {
        "--highlightBg": "#E5E5E5",
        "--cursor": "#333",
        "--bg": "#fff",
        "--svg": "#888",
      };
      this.colorModeName = "light";
    }
    var ua = navigator.userAgent;
    if (
      ua.indexOf("Edge") === -1 &&
      ua.indexOf("Edg") === -1 &&
      ua.indexOf("Trident") === -1
    ) {
      this.isMSEdge = "";
    }
  },
  mounted() {
    if (this.loginForm.username === "") {
      this.$refs.username.focus();
    } else if (this.loginForm.password === "") {
      this.$refs.password.focus();
    }
  },
  methods: {
    checkCapslock(e) {
      const { key, shiftKey } = e;
      if (key.length === 1) {
        this.capsTooltip =
          key &&
          ((key >= "A" && key <= "Z" && !shiftKey) ||
            (key >= "a" && key <= "z" && shiftKey));
      }
    },
    showPwd() {
      if (this.passwordType === "password") {
        this.passwordType = "";
      } else {
        this.passwordType = "password";
      }
      this.$nextTick(() => {
        this.$refs.password.focus();
      });
    },
    handleLogin() {
      var nameOrid = document.getElementById("username").value;
      var password = document.getElementById("password").value;
      if (nameOrid === "" || nameOrid === null) {
        this.$message({
          message: "用户名/学号不能为空",
          center: true,
          type: "error",
        });
      } else if (
        !isNaN(parseInt(nameOrid.substring(0, 1))) &&
        parseInt(nameOrid).toString() !== nameOrid
      ) {
        this.$message({
          message: "用户名不能以数字开头",
          center: true,
          type: "error",
        });
      } else if (
        nameOrid.length != 10 &&
        parseInt(nameOrid).toString() === nameOrid
      ) {
        this.$message({
          message: "学号有误！",
          center: true,
          type: "error",
        });
      } else if (password.length < 8 || password.length > 20) {
        this.$message({
          message: "请输入8~20位的账户密码",
          center: true,
          type: "error",
        });
      } else {
        this.loading = true;
        API.login({ nameOrid, password })
          .then((res) => {
            this.loading = false;
            this.$message({
              message: "登陆成功！页面即将刷新",
              center: true,
              type: "success",
              duration: 750,
              onClose: () => {
                //window.parent.location.reload();
              },
            });
          })
          .catch((err) => {
            this.loading = false;
            alert(err.errorMsg || "用户名或密码错误");
          });
      }
    },
    getOtherQuery(query) {
      return Object.keys(query).reduce((acc, cur) => {
        if (cur !== "redirect") {
          acc[cur] = query[cur];
        }
        return acc;
      }, {});
    },
  },
};
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

div#app {
  $highlightBg: var(--highlightBg);
  $cursor: var(--cursor);
  $bg: var(--bg);
  $svg: var(--svg);

  @supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
    .login-container .el-input input {
      color: $cursor;
    }
  }

  /* reset element-ui css */
  .login-container {
    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;

      input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        color: $cursor;
        height: 47px;
        caret-color: $cursor;

        &:-webkit-autofill {
          box-shadow: 0 0 0px 1000px $highlightBg inset !important;
          -webkit-text-fill-color: $cursor !important;
        }
      }
    }

    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }

    height: 568px;
  }

  .login-container {
    min-height: 100%;
    width: 100%;
    background-color: $bg;
    overflow: hidden;

    .login-form {
      position: relative;
      width: 400px;
      max-width: 100%;
      padding: calc(50vh - 200px) 20px calc(50vh - 100px);
      margin: 0 auto;
      overflow: hidden;
    }

    .tips {
      font-size: 14px;
      color: #fff;
      margin-bottom: 10px;

      span {
        &:first-of-type {
          margin-right: 16px;
        }
      }
    }

    .svg-container {
      padding: 6px 5px 6px 15px;
      color: $svg;
      vertical-align: middle;
      width: 30px;
      display: inline-block;
    }

    .title-container {
      position: relative;

      .title {
        font-size: 26px;
        color: $cursor;
        margin: 0px auto 40px auto;
        text-align: center;
        font-weight: bold;
      }
    }

    .show-pwd {
      position: absolute;
      right: 10px;
      top: 7px;
      font-size: 16px;
      color: $svg;
      cursor: pointer;
      user-select: none;
    }

    .thirdparty-button {
      position: absolute;
      right: 0;
      bottom: 6px;
    }

    .introduction {
      font-size: 15px;
      color: azure;
      margin-left: 800px;
    }
  }
}
</style>
