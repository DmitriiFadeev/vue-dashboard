<template>
  <div class="wrapper">
    <div class="header">
        <div>
            <span>Добавить задачу</span>
            <div>
                <span>Выберете раздел</span>
                <select name="" id="r1">
                    <option 
                    v-for="category in categoties" 
                    :key="category.id" 
                    v-bind:value="category.id"
                    >
                    {{category.title}}
                    </option>
                </select>
            </div>
            <div>
                <span>Выберете наблюдателя</span>
                <select name="" id="n1">
                    <option
                    v-for="item in items"
                    :key="item.id" 
                    v-bind:value="item.user">
                    {{item.user}}
                    </option>
                </select>
            </div>
            <div>
                <span>Введите название задачи</span>
                <input id="t1" type="text">
            </div>
            <div>
                <span>Введите статус задачи</span>
                <input id="s1" type="text">
            </div>
            <div>
                <span>Введите исполнителей</span>
                <div class="performers">
                    <div 
                        v-for="performer in performers"
                        :key="performer.id"
                        class="performer"
                        v-bind:id="'performer_'+performer.id"
                    >
                        <input type="checkbox" v-bind:id="performer.id">
                        <span>{{performer.name}}</span>
                    </div>
                </div>
            </div>
            <input @click="addTask" id="btn" type="button" value="добавить задачу">
        </div>
    </div>
    <div class="content">
        <div 
        v-for="category in categoties" 
        :key="category.id"
        @drop="onDrop($event, category.id)"
        @dragover.prevent
        @dragenter.prevent
        class="content-item">
            <div class="content-item__title">
                <div>{{category.title}}</div>
            </div>
            <div class="content-item__list">
                <div 
                v-for="(item, ind) in items.filter(x => x.categoryId == category.id)"
                :key="item.id"
                @dragstart="onDragStart($event, item)"
                draggable="true"
                class="item">
                    <div class="task">
                        <div class="task-name">
                            <div>{{item.user}}</div>
                            <div>{{item.title}}</div>
                        </div>
                        <div class="avatar">{{ind + 1}}</div>
                    </div>
                    <div class="status">
                        <div>{{item.status}}</div>
                        <div v-for="performer in item.performer" :key="performer.id">
                            <div>{{performer.name}}</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {

  name: 'App',
  data(){
    return {
        items: [
          {
              id: 6,
              user: 'admin1',
              title: 'Бронирование лота в предверии торгов',
              status: 'Ожидание работ',
              performer: [{ id:1, name: 'Антон'}],
              categoryId: 0,
          },
          {
              id: 11,
              user: 'admin2',
              title: 'Дата провередения торгов для комиссиональной торговли',
              status: 'Ожидание работ',
              performer: [{ id:17, name: 'Борис'}],
              categoryId: 0,
          },
          {
              id: 12,
              user: 'admin3',
              title: 'Рефакторинг входа в админку',
              status: 'В работе',
              performer: [{ id:15, name: 'Антон'}],
              categoryId: 1,
          },
          {
              id: 24,
              user: 'admin4',
              title: 'Правки в квитанции по оплате',
              status: 'В работе',
              performer: [{ id:13, name: 'Антон'}],
              categoryId: 1,
          },
          {
              id: 15,
              user: 'admin5',
              title: 'API. Начало',
              status: 'Ожидание тестирования',
              performer: [{ id:11, name: 'Роман'}],
              categoryId: 2,
          },
          {
              id: 26,
              user: 'admin6',
              title: 'Раздел "продавцы" на yii',
              status: 'Ожидание тестирования',
              performer: [{ id:13, name: 'Вячеслав'}],
              categoryId: 2,
          },
          {
              id: 13,
              user: 'admin7',
              title: 'Ошибка формирования протокола',
              status: 'Парное тестирование',
              performer: [{ id:1, name: 'Александр'}],
              categoryId: 3,
          },
          {
              id: 28,
              user: 'admin8',
              title: 'Мои данные, Сокращенное',
              status: 'Тестирование',
              performer: [{ id:3, name: 'Александр'}],
              categoryId: 3,
          },
          {
              id: 10,
              user: 'admin9',
              title: 'Ошибки в дате окончания подачи заявки',
              status: 'Ревизия функциональности',
              performer: [{ id:1, name: 'Антон'}],
              categoryId: 4,
          },
          {
              id: 29,
              user: 'admin0',
              title: 'Ошибка 404',
              status: 'Ревизия функциональности',
              performer: [{ id:3, name: 'Павел'}],
              categoryId: 4,
          }
      ],
      categoties: [
          {
              id: 0,
              title: 'К работе'
          },
          {
              id: 1,
              title: 'В работе'
          },
          {
              id: 2,
              title: 'К тестированию'
          },
          {
              id: 3,
              title: 'В тестировании'
          },
          {
              id: 4,
              title: 'Почти готово'
          }
      ],
      performers: [
          {
              id:1, name: 'Антон'
          },
          {   
              id:2, name: 'Георгий'
          },
          {   
              id:3, name: 'Алексей'
          },
          {   
              id:4, name: 'Георгий'
          },
          {   
              id:5, name: 'Константин'
          },
          {   
              id:6, name: 'Павел'
          },
      ]
    }
  },
  methods: {
      onDragStart(e, item){
          e.dataTransfer.dropEffect = 'move'
          e.dataTransfer.effectAllowed = 'move'
          e.dataTransfer.setData('itemId',item.id.toString())
      },

      onDrop(e, categoryId){
          const itemId = parseInt(e.dataTransfer.getData('itemId'))
          this.items = this.items.map(x => {
              if(x.id == itemId){
                x.categoryId = categoryId
              }
            return x
          })
      },

      addTask(){
        const category = document.querySelector('#r1').value
        const user = document.querySelector('#n1').value
        const title = document.querySelector('#t1').value
        const status = document.querySelector('#s1').value
        const performers = document.querySelector('.performers')

        let isp = performers.querySelectorAll('input');

        let s = [];
        
        isp.forEach(element => {
            if(element.checked){
                let per = document.querySelector(`#performer_${element.id}`)
                let id = per.querySelector('input').id
                let name = per.querySelector('span').innerHTML
                s.push({id, name})
            }
        });
        
        this.items.push({
            id: Math.floor(Math.random() * 3),
            user: user,
            title: title,
            status: status,
            performer: s,
            categoryId: category,
        })
      }
  },
}
</script>

<style>
*{
    box-sizing: border-box;
}
body{
    margin: 0;
    padding: 0;
}
.header{
    display: flex;
    flex-direction: column;
}
.header .name{
    font-size: 16px;
    color: #979797;
    text-transform: uppercase;
}
.header .board{
    font-size: 18px;
    color: black;
    margin: 15px 0;
}
.header .filter{
    font-size: 14px;
    color: black;
}
.header>div>div{
    margin: 5px 0;
}
.header>div>div>span{
    margin-right: 10px;
}
.content{
    border-top: 1px solid #979797;
    display: flex;
    justify-content: space-between;
    padding: 15px 10px 0 10px;
    background-color: #fff;
    margin-top: 15px;
    height: 700px;
}
.content .content-item{
    width: 20%;
    padding: 10px;
}
.content .content-item .content-item__title{
    display: flex;
}
.content .content-item .content-item__title span{
    font-weight: bold;
    margin-right: 10px;
}
.content .content-item .content-item__title div{

}
.content .content-item .content-item__list{
    background-color: hsl(0, 26%, 95%);
    height: 100%;
    margin-top: 15px;
}
.content .content-item .content-item__list .item{
    display: flex;
    flex-direction: column;
    border: 1px solid #979797;
    border-bottom: none;
    background-color: #fff;
}
.content .content-item .content-item__list .item:last-child{
    border-bottom: 1px solid #979797;
}
.content .content-item .content-item__list .item .task{
    display: flex;
    justify-content: space-between;
    padding: 10px;
}
.content .content-item .content-item__list .item .task .performer{
    width: 10%;
}
.content .content-item .content-item__list .item .task .task-name{

}
.content .content-item .content-item__list .item .task .avatar{
    width: 20px;
    height: 20px;
    background-color: pink;
}
.content .content-item .content-item__list .item .status{
    display: flex;
    flex-direction: column;
    padding: 10px;
}
.content .content-item .content-item__list .item .status div{
    font-size: 14px;
    line-height: 21px;
}
</style>
