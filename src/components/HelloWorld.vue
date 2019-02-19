<template>
  <div class="work">
    <h1>todos</h1>
    <div class="search">
      <span class="xuanz" @click="oldbady" :class="{'active':every,'disnone':this.total.length===0}">✔</span>
      <input
        type="text"
        placeholder="what needs to be done?"
        class="new-todo"
        autofocus
        v-on:keyup.enter="submit"
        v-model="search"
      >
      <ul class="todo-list">
        <li v-for="(item,index) in totals(setkeys)" :key="item.id">
          <div class="view">
            <span class="Checkbox" @click="checked(index)">{{item.completed?"✔":''}}</span>
            <label  @dblclick="dbclick(index)" :class="{'active':item.completed}" ref="clickoss" >{{item.title}}</label>
            <input class="label" v-model="item.title" type="text" @blur="searchblur(index)"  v-on:keyup.enter="searchblur(index)"  ref="clicko">
            <span class="destroy" @click="destroy(index)"></span>
          </div>
        </li>
      </ul>
      <div class="foot" v-show='this.total.length!==0'>
        <span class="fleater">
          <span>{{totals('active').length}}</span> items left
        </span>
        <ul>
          <li>
            <a :class="{'active':all}" @click="All">All</a>
          </li>
          <li>
            <a :class="{'active':active}" @click="actives">Active</a>
          </li>
          <li>
            <a :class="{'active':completed}" @click="completeds">Completed</a>
          </li>
          <a class="clear" v-show='totals ("completed").length' @click="removerloca">Clear completed</a>
        </ul>
      </div>
    </div>
    <footer class="info">
      <p>Double-click to edit a todo</p>
      <p>
        Created by
        <a href="http://sindresorhus.com">Sindre Sorhus</a>
      </p>
      <p>
        Part of
        <a href="http://todomvc.com">TodoMVC</a>
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  data: () => ({
    search: '',
    total: [],
    every: false,
    all: true,
    active: false,
    completed: false,
    setkeys: ''
  }),
  created () {
    this.settatal()
  },
  mounted () {
    this.setevery()
  },
  // 双击
  methods: {
    dbclick (index) {
      this.$refs.clickoss[index].style.display = 'none'
      this.$refs.clicko[index].style.display = 'block'
      this.$refs.clicko[index].focus()
    },
    // 筛选total数据
    totals (keys) {
      if (keys === 'active') {
        let total = (JSON.parse(localStorage.getItem('total')) || []).filter(item => {
          return item.completed === false
        })
        return total
      } else if (keys === 'completed') {
        let total = (JSON.parse(localStorage.getItem('total')) || []).filter(item => {
          return item.completed === true
        })
        return total
      } else {
        return this.total
      }
    },
    // all的数据
    All () {
      this.all = true
      this.active = false
      this.completed = false
      this.setkeys = ''
    },
    // 未完成的数据
    actives () {
      this.all = false
      this.active = true
      this.completed = false
      this.setkeys = 'active'
    },
    // 已完成
    completeds () {
      this.all = false
      this.active = false
      this.completed = true
      this.setkeys = 'completed'
    },
    // 全选
    setevery () {
      if (this.total.length === 0) {
        this.every = false
        return
      }
      this.every = this.total.every(item => {
        return item.completed
      })
    },
    // 获取localStorage的值
    settatal () {
      this.total = JSON.parse(localStorage.getItem('total')) || []
    },
    // 输入框提交
    submit () {
      if (!this.search.trim()) {
        console.log('请输入')
        return
      }
      this.total = JSON.parse(localStorage.getItem('total')) || []
      var data = new Date().getTime()
      console.log(data)
      var list = {
        id: data,
        title: this.search,
        completed: false
      }
      this.total.push(list)
      localStorage.setItem('total', JSON.stringify(this.total))
      this.search = ''
      this.setevery()
    },
    // 勾选完成
    checked (id) {
      this.total[id].completed = !this.total[id].completed
      localStorage.setItem('total', JSON.stringify(this.total))
      this.setevery()
    },
    // 删除一条数据
    destroy (id) {
      this.total.splice(id, 1)
      localStorage.setItem('total', JSON.stringify(this.total))
    },
    // 反选
    oldbady () {
      if (!this.total) {
        this.every = false
        return
      }
      let every = this.total.every(item => {
        return item.completed
      })
      this.every = !every
      if (every) {
        this.total.forEach(item => {
          item.completed = false
        })
      } else {
        this.total.forEach(item => {
          item.completed = true
        })
      }
      localStorage.setItem('total', JSON.stringify(this.total))
    },
    // 删除完成的数据
    removerloca () {
      this.total = this.total.filter(item => {
        return item.completed === false
      })
      localStorage.setItem('total', JSON.stringify(this.total))
    },
    // 输入框失去焦点
    searchblur (index) {
      this.$nextTick(function () {
        this.$refs.clickoss[index].style.display = 'block'
        this.$refs.clicko[index].style.display = 'none'
        localStorage.setItem('total', JSON.stringify(this.total))
      })
    }
  }
}
</script>
<style lang='less'>
li {
  list-style: none;
}
.work {
  h1 {
    font-size: 100px;
    font-weight: 500;
    text-align: center;
    color: rgba(175, 47, 47, 0.15);
  }
  .search {
    width: 550px;
    margin: 0 auto;
    background-color: #fff;
    position: relative;
    .xuanz{
      position: absolute;
      font-size: 26px;
      font-weight: 700;
      left: 20px;
      top: 15px;
      color: #ccc;
      z-index: 1000;
      &.active{
        color: #cc9a9a;
      }
      &.disnone{
        display: none;
      }
    }
    .new-todo {
      padding: 16px 16px 16px 60px;
      border: none;
      box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
      position: relative;
      margin: 0;
      width: 100%;
      font-size: 24px;
      font-family: inherit;
      font-weight: inherit;
      line-height: 1.4em;
      color: inherit;
      box-sizing: border-box;
      outline: none;
    }
    .todo-list {
      .view:hover .destroy{
        opacity: .8;
      }
      .view {
        background-color: #fff;
        position: relative;
        border-bottom: 1px solid #ddd;
        padding-bottom: 1px;
        box-sizing: border-box;
        background-color: transparent;
        overflow: hidden;
        padding-left: 50px;
        .Checkbox {
          position: absolute;
          height: 30px;
          width: 30px;
          left: 10px;
          top: 50%;
          transform: translateY(-50%);
          border-radius: 50%;
          color: aquamarine;
          border: 1px solid #ededed;
          text-align: center;
          line-height: 40px;
          font-weight: 700;
          font-size: 25px;
        }
        label,
        .label {
          word-break: break-all;
          padding: 15px 15px 15px 20px;
          line-height: 28px;
          transition: color 0.4s;
          box-sizing: border-box;
          border: none;
          font-weight: 600;
          font-size: 18px;
          color: #777;
           background-color: #fff;
           display: block;
        }
        .active{
          color: #ddd;
          text-decoration: line-through;
        }
        .label {
          width: 100%;
          float: left;
          display: none;
        }
        .destroy {
          position: absolute;
          right: 30px;
          top: 50%;
          border: none;
          border-radius: 50%;
          width: 30px;
          height: 30px;
          transform: translateY(-50%);
          opacity: 0;
        }
        .destroy:hover  {
          opacity: 1;
        }
        .destroy::after,
        .destroy::before {
          content: "";
          position: absolute;
          left: 50%;
          top: 50%;
          width: 3px;
          height: 20px;
          background-color: #cc9a9a;
        }
        .destroy::before {
          transform: translate(-50%, -50%) rotate(-45deg);
        }
        .destroy::after {
          transform: translate(-50%, -50%) rotate(45deg);
        }
      }
    }
  }
  .info {
    padding-top: 40px;
    > p {
      text-align: center;
      > a {
        color: #4d4d4d;
        text-decoration: none;
      }
      a:hover {
        text-decoration: underline;
      }
    }
  }
  .foot {
    color: #777;
    padding: 10px 15px;
    height: 20px;
    text-align: center;
    background-color: #fff;
    font-size: 12px;
    .fleater {
      float: left;
    }
    ul {
      li {
        display: inline-block;
        a {
          text-decoration: none;
          color: #777;
          padding: 3px 7px;
          border-radius: 3px;
        }
        .active {
          border: 1px solid rgba(175, 47, 47, 0.2);
        }
      }
    }
    .clear {
      position: absolute;
      right: 15px;
      bottom: 15px;
      color: #777;
      text-decoration: none;
      cursor: pointer;
    }
    .clear:hover {
      text-decoration: underline;
    }
  }
}
</style>
