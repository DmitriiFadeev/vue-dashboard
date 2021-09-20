<template>
  <div class="wrapper">
    <div class="header">
        <div class="name">NAME</div>
        <div class="board">Kanban board</div>
        <div class="filter">
            <span>quick filtres:</span>
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
                <span class="count">3</span>
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
                        <div class="performer"></div>
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
import { ref } from 'vue'

export default {

  name: 'App',
  setup() {
      const items = ref([
          {
              id: 6,
              user: 'admin1',
              title: 'Бронирование лота в предверии торгов',
              status: 'Ожидание работ',
              performer: [{ id:1, name: 'Антон'},{id:2, name: 'Георгий'}],
              categoryId: 0,
          },
          {
              id: 11,
              user: 'admin2',
              title: 'Дата провередения торгов для комиссиональной торговли',
              status: 'Ожидание работ',
              performer: [{ id:17, name: 'Борис'},{id:27, name: 'Георгий'}],
              categoryId: 0,
          },
          {
              id: 12,
              user: 'admin3',
              title: 'Рефакторинг входа в админку',
              status: 'В работе',
              performer: [{ id:15, name: 'Антон'},{id:24, name: 'Алексей'}],
              categoryId: 1,
          },
          {
              id: 24,
              user: 'admin4',
              title: 'Правки в квитанции по оплате',
              status: 'В работе',
              performer: [{ id:13, name: 'Антон'},{id:24, name: 'Максим'}],
              categoryId: 1,
          },
          {
              id: 15,
              user: 'admin5',
              title: 'API. Начало',
              status: 'Ожидание тестирования',
              performer: [{ id:11, name: 'Роман'},{id:23, name: 'Георгий'}],
              categoryId: 2,
          },
          {
              id: 26,
              user: 'admin6',
              title: 'Раздел "продавцы" на yii',
              status: 'Ожидание тесстирования',
              performer: [{ id:13, name: 'Вячеслав'},{id:22, name: 'Илья'}],
              categoryId: 2,
          },
          {
              id: 13,
              user: 'admin7',
              title: 'Ошибка формирования протокола',
              status: 'Парное тестирование',
              performer: [{ id:1, name: 'Александр'},{id:2, name: 'Инна'}],
              categoryId: 3,
          },
          {
              id: 28,
              user: 'admin8',
              title: 'Мои данные, Сокращенное',
              status: 'Тестирование',
              performer: [{ id:3, name: 'Александр'},{id:4, name: 'Юлия'}],
              categoryId: 3,
          },
          {
              id: 10,
              user: 'admin9',
              title: 'Ошибки в дате окончания подачи заявки',
              status: 'Ревизия функциональности',
              performer: [{ id:1, name: 'Антон'},{id:2, name: 'Георгий'}],
              categoryId: 4,
          },
          {
              id: 29,
              user: 'admin0',
              title: 'Ошибка 404',
              status: 'Ревизия функциональности',
              performer: [{ id:3, name: 'Павел'},{id:4, name: 'Алексей'}],
              categoryId: 4,
          }
      ])

      const categoties = ref([
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
      ])

      const onDragStart = (e, item) => {
          e.dataTransfer.dropEffect = 'move'
          e.dataTransfer.effectAllowed = 'move'
          e.dataTransfer.setData('itemId',item.id.toString())
      }

      const onDrop = (e, categoryId) => {
          const itemId = parseInt(e.dataTransfer.getData('itemId'))
          items.value = items.value.map(x => {
              if(x.id == itemId)
                  x.categoryId = categoryId
            return x
          })
          console.log('items', items)
      }

      return {
          items, 
          categoties,
          onDragStart,
          onDrop
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
    background-color: red;
}
.content .content-item .content-item__list .item .status{
    display: flex;
    flex-direction: column;
    padding-left: 10%;
}
.content .content-item .content-item__list .item .status div{
    font-size: 14px;
    line-height: 21px;
}
</style>
