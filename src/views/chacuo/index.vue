<template>
  <div class="app-container documentation-container">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="js" name="first">
        <iframe style="width:1000px;height:500px;" src="/jshint.html" />
      </el-tab-pane>
      <el-tab-pane label="xml" name="second">
        <iframe style="width:1000px;height:500px;" src="/Fast XML Parser.html" />
      </el-tab-pane>
      <el-tab-pane label="css" name="third">
        <iframe style="width:1000px;height:500px;" src="http://localhost:8081/css-validator/#validate_by_input" />
      </el-tab-pane>
    </el-tabs>
    <!--
    <div class="editor">
      <input v-model="message" placeholder="请在此输入问题标题" class="title-input">
      <div class="editor-all">
        <editor-menu-bar v-slot="{ commands, isActive }" :editor="editor">
          <div class="menubar">
            <div class="search">
              <div class="search-inputs">
              <input
                ref="search"
                @keydown.enter.prevent="editor.commands.find(searchTerm)"
                placeholder="Search …"
                type="text"
                v-model="searchTerm"
              />
              <input
                @keydown.enter.prevent="editor.commands.replace(replaceWith)"
                placeholder="Replace …"
                type="text"
                v-model="replaceWith"
              />
              </div>
              <div class="search-buttons">
                <button class="button" style="font-size:0.8em;line-height:24px;font-weight:500" @click="editor.commands.find(searchTerm)">
                  Find
                </button>
                <button class="button" style="font-size:0.8em;line-height:24px;font-weight:500" @click="editor.commands.clearSearch()">
                  Clear
                </button>
                <button class="button" style="font-size:0.8em;line-height:24px;font-weight:500" @click="editor.commands.replace(replaceWith)">
                  Replace
                </button>
                <button class="button" style="font-size:0.8em;line-height:24px;font-weight:500" @click="editor.commands.replaceAll(replaceWith)">
                  Replace All
                </button>
              </div>
            </div>

            <button class="menubar__button" @click="commands.undo">
              <icon name="undo" />
            </button>

            <button class="menubar__button" @click="commands.redo">
              <icon name="redo" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.bold() }" @click="commands.bold">
              <icon name="bold" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.italic() }" @click="commands.italic">
              <icon name="italic" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.strike() }" @click="commands.strike">
              <icon name="strike" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.underline() }" @click="commands.underline">
              <icon name="underline" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.paragraph() }" @click="commands.paragraph">
              <icon name="paragraph" />
            </button>

            <button class="menubar__button" style="line-height:12px;font-weight:500"
              :class="{ 'is-active': isActive.heading({ level: 1 }) }" @click="commands.heading({ level: 1 })">
              H1
            </button>

            <button class="menubar__button" style="line-height:12px;font-weight:500"
              :class="{ 'is-active': isActive.heading({ level: 2 }) }" @click="commands.heading({ level: 2 })">
              H2
            </button>

            <button class="menubar__button" style="line-height:12px;font-weight:500"
              :class="{ 'is-active': isActive.heading({ level: 3 }) }" @click="commands.heading({ level: 3 })">
              H3
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.bullet_list() }" @click="commands.bullet_list">
              <icon name="ul" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.ordered_list() }" @click="commands.ordered_list">
              <icon name="ol" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.blockquote() }" @click="commands.blockquote">
              <icon name="quote" />
            </button>

            <button class="menubar__button"
              :class="{ 'is-active': isActive.code_block() }" @click="commands.code_block">
              <icon name="code" />
            </button>

            <button class="menubar__button" @click="commands.horizontal_rule">
              <icon name="hr" />
            </button>

            <button class="menubar__button" @click="showImagePrompt(commands.image)">
              <icon name="image" />
            </button>

            <button class="menubar__button" @click="commands.createTable({rowsCount: 3, colsCount: 3, withHeaderRow: false })">
              <icon name="table" />
            </button>

            <span v-if="isActive.table()">
              <button class="menubar__button" @click="commands.deleteTable">
                <icon name="delete_table" />
              </button>
              <button class="menubar__button" @click="commands.addColumnBefore">
                <icon name="add_col_before" />
              </button>
              <button class="menubar__button" @click="commands.addColumnAfter">
                <icon name="add_col_after" />
              </button>
              <button class="menubar__button" @click="commands.deleteColumn">
                <icon name="delete_col" />
              </button>
              <button class="menubar__button" @click="commands.addRowBefore">
                <icon name="add_row_before" />
              </button>
              <button class="menubar__button" @click="commands.addRowAfter">
                <icon name="add_row_after" />
              </button>
              <button class="menubar__button" @click="commands.deleteRow">
                <icon name="delete_row" />
              </button>
              <button class="menubar__button" @click="commands.toggleCellMerge">
                <icon name="combine_cells" />
              </button>
            </span>
          </div>
        </editor-menu-bar>
        <editor-content class="editor__content" :editor="editor" />
      </div>
      <div>
        <h3 style="margin-top:2rem;margin-left:1rem;font-size:1rem;">标签管理</h3>
        <p style="margin:0 1rem;font-size:0.8rem;color:#888;">彩色为已添加的标签，灰色为待添加的标签，正确添加标签有助于检索，利于问题被解答哦～</p>
        <div class="flags">
          <button class="flags-btn" :class="{'flags-wxml':active.wxml}" @click="changeFlag('wxml')">wxml</button>
          <button class="flags-btn" :class="{'flags-wxss':active.wxss}" @click="changeFlag('wxss')">wxss</button>
          <button class="flags-btn" :class="{'flags-less':active.less}" @click="changeFlag('less')">less</button>
          <button class="flags-btn" :class="{'flags-js':active.js}" @click="changeFlag('js')">js</button>
          <button class="flags-btn" :class="{'flags-ts':active.ts}" @click="changeFlag('ts')">ts</button>
          <button class="flags-btn" :class="{'flags-json':active.json}" @click="changeFlag('json')">json</button>
          <button class="flags-btn" :class="{'flags-component':active.component}" @click="changeFlag('component')">组件</button>
          <button class="flags-btn" :class="{'flags-api':active.api}" @click="changeFlag('api')">API</button>
          <button class="flags-btn" :class="{'flags-wxcloud':active.wxcloud}" @click="changeFlag('wxcloud')">云函数</button>
        </div>
      </div>
    </div>
    -->
    
    
    <li class="find"><router-link to="/clipboard">还没有解决问题？直接去提问吧</router-link></li>
  </div>
</template>

<script>
import DropdownMenu from '@/components/Share/DropdownMenu'

import javascript from 'highlight.js/lib/languages/javascript'
import typescript from 'highlight.js/lib/languages/typescript'
import css from 'highlight.js/lib/languages/css'
import less from 'highlight.js/lib/languages/less'
import xml from 'highlight.js/lib/languages/xml'
import json from 'highlight.js/lib/languages/json'
import c from 'highlight.js/lib/languages/c'
import cpp from 'highlight.js/lib/languages/cpp'
import c_like from 'highlight.js/lib/languages/c-like'
import arduino from 'highlight.js/lib/languages/arduino'

import Icon from '@/components/Icon'
import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
import {
  Blockquote,
  CodeBlockHighlight,
  HardBreak,
  Heading,
  HorizontalRule,
  Image,
  OrderedList,
  BulletList,
  ListItem,
  TodoItem,
  TodoList,
  Bold,
  Italic,
  Link,
  Table,
  TableHeader,
  TableCell,
  TableRow,
  Strike,
  Underline,
  History,
  Search,
  TrailingNode
} from 'tiptap-extensions'

export default {
  name: 'Documentation',
  components: {
    EditorContent,
    EditorMenuBar,
    Icon,
    DropdownMenu
  },
  data() {
    return {
      activeName: 'second',
      editor: new Editor({
        extensions: [
          new Blockquote(),
          new BulletList(),
          new CodeBlockHighlight({
            languages: {
              javascript, typescript, css, less, xml, json, c, cpp, c_like, arduino
            }
          }),
          new HardBreak(),
          new Heading({ levels: [1, 2, 3] }),
          new HorizontalRule(),
          new Image(),
          new ListItem(),
          new OrderedList(),
          new TodoItem({
            nested: true
          }),
          new TodoList(),
          new Link(),
          new Bold(),
          new Italic(),
          new Strike(),
          new Underline(),
          new History(),
          new Search({
            disableRegex: false
          }),
          new TrailingNode({
            node: 'paragraph',
            notAfter: ['paragraph']
          }),
          new Table({
            resizable: true
          }),
          new TableHeader(),
          new TableCell(),
          new TableRow()
        ],
        content: `
          <h2>
            Hi there,
          </h2>
          <p>
            this is a very <em>basic</em> example of tiptap.
          </p>
          <pre><code>body { display: none; }</code></pre>
          <ul>
            <li>
              A regular list
            </li>
            <li>
              With regular items
            </li>
          </ul>
          <blockquote>
            It's amazing 👏
            <br />
            – mom
          </blockquote>
        `,
        onBlur: (e) => {
          console.log(this.editor.getJSON())
        }
      }),
      active: {
        wxml: false,
        wxss: false,
        less: false,
        js: false,
        ts: false,
        json: false,
        component: false,
        api: false,
        wxcloud: false
      }
    }
  },
  beforeDestroy() {
    this.editor.destroy()
  },
  methods: {
    showImagePrompt(command) {
      const src = prompt('Enter the url of your image here')
      if (src !== null) {
        command({ src })
      }
    },
    changeFlag(t) {
      this.active[t] = !this.active[t]
    },
    handleClick(tab, event) {
      console.log(tab, event);
    }
  }
}
</script>

<style lang="scss">
@import 'highlight.js/scss/tomorrow-night-eighties.scss';
@import '@/assets/sass/variables.scss';
@media (min-width: 800px) {
  .search{
    display:flex;
  }
}
.flags {
  display: flex;
  flex-wrap: wrap;
  margin: 0.5rem 1rem;
  .flags-btn {
    padding: 0.5rem 0.75rem;
    overflow: hidden;
    border-radius: 0.5rem;
    border: none;
    color: #888;
    background: #eee;
    transition: all .3s ease;
    margin-right: 0.5rem;
    letter-spacing: 1px;
    font-size: 0.8rem;
  }
  .flags-wxml {
    color: #eee;
    background: #2ecc71;
    transition: all .3s ease;
  }
  .flags-wxss {
    color: #eee;
    background: #3498db;
    transition: all .3s ease;
  }
  .flags-less {
    color: #eee;
    background: #2980b9;
    transition: all .3s ease;
  }
  .flags-js {
    color: #eee;
    background: #f1c40f;
    transition: all .3s ease;
  }
  .flags-ts {
    color: #eee;
    background: #e67e22;
    transition: all .3s ease;
  }
  .flags-json {
    color: #eee;
    background: #e74c3c;
    transition: all .3s ease;
  }
  .flags-component {
    color: #eee;
    background: #9b59b6;
    transition: all .3s ease;
  }
  .flags-api {
    color: #eee;
    background: #f39c12;
    transition: all .3s ease;
  }
  .flags-wxcloud {
    color: #eee;
    background: #e67e22;
    transition: all .3s ease;
  }
}
.editor-all {
  background: #eeeeee;
  border-radius: 1rem;
  overflow: hidden;
  margin: 1rem 0;
}
.menubar {
  padding: 0.5rem;
  background: #dddddd;
}
.editor__content {
  padding: 1rem;
}
.search {
  overflow: hidden;
  padding: 0.25rem;
  width: 100%;
  .search-inputs {
    display: flex;
    flex-wrap: wrap;
    flex: 1;
    input {
      padding: 0.25rem 0.5rem;
      border: 0;
      border-radius: 3px;
      margin-right: 0.2rem;
      font: inherit;
      font-size: 0.8rem;
      width: 20%;
      flex: 1;
      margin: 0.25rem;
      border-radius: 0.5rem;
    }
  }
  .search-buttons {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-end;
    .button{
      margin: 0.25rem;
      border-radius: 0.5rem;
      overflow: hidden;
    }
  }
}
.find {
  background: rgba(255, 213, 0, 0.5);
}
ul[data-type="todo_list"] {
  padding-left: 0;
}
li[data-type="todo_item"] {
  display: flex;
  flex-direction: row;
}
.todo-checkbox {
  border: 2px solid $color-black;
  height: 0.9em;
  width: 0.9em;
  box-sizing: border-box;
  margin-right: 10px;
  margin-top: 0.3rem;
  user-select: none;
  -webkit-user-select: none;
  cursor: pointer;
  border-radius: 0.2em;
  background-color: transparent;
  transition: 0.4s background;
}
.todo-content {
  flex: 1;
  > p:last-of-type {
    margin-bottom: 0;
  }
  > ul[data-type="todo_list"] {
    margin: .5rem 0;
  }
}
li[data-done="true"] {
  > .todo-content {
    > p {
      text-decoration: line-through;
    }
  }
  > .todo-checkbox {
    background-color: $color-black;
  }
}
li[data-done="false"] {
  text-decoration: none;
}
.title-input{
  border: none;
  width: 100%;
  font-size: 32px;
  font-weight: 700;
  padding: 16px 8px;
}
</style>

<style lang="scss" scoped>
.documentation-container {
  margin: 50px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;

  .document-btn {
    flex-shrink: 0;
    display: block;
    cursor: pointer;
    background: black;
    color: white;
    height: 60px;
    width: 200px;
    margin-bottom: 16px;
    line-height: 60px;
    font-size: 20px;
    text-align: center;
  }
}

.find{
    font-size: 13px;
    color: rgb(102, 189, 230);
    margin-left: 800px;
  }
</style>
