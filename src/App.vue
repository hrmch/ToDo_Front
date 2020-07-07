<template>
  <v-app>
    <!--サイドメニュー-->
    <v-navigation-drawer app v-model="drawer" clipped>
      <v-container>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="title grey--text text--darken-2">
              メニュー
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <v-list nav>
          <v-list-item v-for="nav_list in nav_lists" :key="nav_list.name">
            <v-list-item-icon>
              <v-icon>{{nav_list.icon}}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{nav_list.name}}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-container>
    </v-navigation-drawer>

    <!--ヘッダー-->
    <v-app-bar color="primary" dark app clipped-left>
      <v-app-bar-nav-icon @click="drawer=!drawer"></v-app-bar-nav-icon>
 
      <v-toolbar-title>ToDo管理ハイパー(仮)</v-toolbar-title>
      <v-spacer></v-spacer>
       
      <v-toolbar-items>
        <v-btn text>ボタン</v-btn>

        <v-menu offset-y>
          <template v-slot:activator="{on}">
            <v-btn v-on="on" text>ドロップダウンボタン<v-icon>mdi-menu-down</v-icon></v-btn>
          </template>
          <v-list>
            <v-subheader>機能群</v-subheader>
            <v-list-item v-for="func in funcs" :key="func.name">
              <v-list-item-icon>
                <v-icon>{{func.icon}}</v-icon>
              </v-list-item-icon>  
              <v-list-item-content>
                <v-list-item-title>{{func.name}}</v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-toolbar-items>
    </v-app-bar>

    <!--メイン画面-->
    <v-content>
      <v-container text-center wrap md12>
        <v-card-title>
          <v-text-field solo @keydown.prevent.enter="addTask()"
            placeholder="タスクを入力してください"
            v-model="task"
          ></v-text-field>
        </v-card-title>
        
        <v-expansion-panels popout>
          <v-expansion-panel
            v-for="(item,i) in items" :key="i"
          >
            <v-expansion-panel-header disable-icon-rotate>
              <!--ここにテキストフィールド？-->
              {{item.task}}
              <template v-slot:actions>
                <v-icon color="success" @click="completeTask(item)">mdi-check</v-icon>
              </template>
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-col>
                <v-text-field
                  label="タグ"
                  v-model="tag"
                >
                </v-text-field>
              </v-col>
              <v-col>
                <v-textarea
                  label="詳細"
                  v-model="detail"
                ></v-textarea>
              </v-col>
              <v-col align="right">
                <v-btn @click="updateTask(i)">更新</v-btn>
              </v-col>
              <v-col align="right">
                <v-icon color="error" @click="deleteTask(item)">mdi-delete</v-icon>
              </v-col>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-container>
    </v-content>

    <!--フッター-->
    <v-footer color="primary" dark app>
      Todo管理ハイパー(仮)
    </v-footer>

  </v-app>
</template>

<script>
export default {
  data(){
    return{
      drawer: null,
      funcs:[
        {name: '機能１', icon: 'mdi-function'},
        {name: '機能２', icon: 'mdi-function'},
        {name: '機能３', icon: 'mdi-function'}
      ],
      nav_lists:[
        {name: 'メニュー１', icon: 'mdi-function'},
        {name: 'メニュー２', icon: 'mdi-function'},
        {name: 'メニュー３', icon: 'mdi-function'}
      ],
      items: [],
      task: "",
    }
  },
  methods: {
    addTask: function() {
      if (!this.task) {
        alert("作業を入力してください");
        return;
      }
      this.items.push({
        "task": this.task,
        "tag": "",
        "detail": "",
      });
      this.task = "";
    },
    updateTask: function(i) {
      this.$set(this.items, i, {"task": this.items[i].task, "tag":this.tag, "detail":this.detail});
    },
    deleteTask: function(item) {
      this.items.splice(this.items.indexOf(item), 1);
    },
    completeTask: function(item) {
      this.items.splice(this.items.indexOf(item), 1);
    }
  }    
};
</script>
