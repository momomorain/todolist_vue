<script>


export default {
  data() {
    return {
      addText: '',
      todoList: [],
      selectedTab: '',
    };
  },
  methods: {
    addItemList() {
      let listID = 0;
      if (this.todoList.length !== 0) {
        listID = this.todoList[this.todoList.length - 1].id;
      }
      let text = this.addText;
      if (text != '') {
        const item = {
          id: listID + 1,
          check: false,
          text: text,
          editSwitch: false,
          editMsg: '',
        }
        this.todoList.push(item);
        console.log(this.todoList);
        localStorage.setItem('msg', JSON.stringify(this.todoList));
        this.addText = "";
      };
    },
    getInputValue(e) {
      this.addText = e.target.value;
      e.target.value = '';
    },
    edit(id) {
      console.log(id);
      const content = prompt('修改文字');
      if (content != '' || content == null) {
        this.todoList[id].text = content;
      }
    },
    deleteArr(id) {
      this.todoList.splice(id, 1);
      localStorage.setItem('msg', JSON.stringify(this.todoList));
    },
    changTab(string) {
      this.selectedTab = string;
    },
    editMsg(item) {
      console.log(item);
      item.editSwitch = !item.editSwitch;
      if (item.editMsg !== '') {
        item.text = item.editMsg;
      }
      if (item.editSwitch === true) {
        item.editMsg = item.text;
      }
      localStorage.setItem('msg', JSON.stringify(this.todoList))
    }
  },
  mounted() {
    // 當網頁開啟後，先執行裡面的JS
    // 先把資料從localStorage 特定Key拿出資料來，丟入msgArr裡面
    if (localStorage.getItem('msg')) {
      this.todoList = JSON.parse(localStorage.getItem('msg'));
    }

  },
  computed: {
    // 預處理拿到的資料有暫存功能，所以我們可以拿整包資料，利用判斷式把資料篩選
    filterData() {
      
      if (this.selectedTab === 'all') {
        return this.todoList;
      } else if (this.selectedTab === 'is-todo') {
        return this.todoList.filter(list => list.check === true);
      } else if (this.selectedTab === 'not-todo') {
        return this.todoList.filter(list => list.check === false);
      }
    }
  }
}
</script>

<template>
  <div class="w-screen h-screen flex justify-center items-center bg-gray-800 ">
    <div class="w-3/4 h-3/4 flex-col justify-center items-center p-5 bg-gradient-to-l from-gray-500 to-zinc-800">
      <div class="p-5 flex items-center">
        <input v-model="addText" type="text" class="w-[250px] mr-2 text-black">
        <button class="button bg-zinc-600" @click="addItemList">新增</button>
      </div>
      <div class="flex gap-4">
        <button type="button" class="button bg-zinc-500" @click="selectedTab = 'all'">全部</button>
        <button type="button" class="button bg-zinc-500" @click="selectedTab = 'is-todo'">已執行</button>
        <button type="button" class="button bg-zinc-500" @click="selectedTab = 'not-todo'">未執行</button>
        <button type="button" class="button bg-zinc-600">記住我?</button>
      </div>
      <table class="w-full">
        <tr>
          <th class="text-white text-center">執行</th>
          <th class="text-white text-center">事項</th>
          <th class="text-white text-center">功能</th>
        </tr>
        <tr v-for="(item, index) in filterData" :key="index">
          <td class="text-white text-center mt-3">
            <input v-model="todoList[index].check" type="checkbox" class="ml-18">
          </td>
          <td v-if="item.editSwitch === false" class="text-white text-center ">{{ item.text }}</td>
          <td v-else class="text-black text-center ">
            <input v-model="item.editMsg" type="text">
          </td>


          <td class="text-white text-center">
            <!-- <button class="button bg-zinc-500" @click="edit(index)">編輯</button> -->
            <button class="button bg-zinc-500" @click="editMsg(item)">編輯</button>
            <button class="button bg-zinc-500 ml-2" @click="deleteArr(index)">刪除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
.button {
  @apply rounded-lg px-4 py-2 text-white text-lg shadow-md transition duration-300 ease-in-out;
}

.button:hover {
  @apply bg-zinc-700;
}
</style>


