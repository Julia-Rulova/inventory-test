<template>
  <section class="app">
    <div class="app__main-container">
      <div class="section description">
        <img class="description__img" src="../assets/images/blur-img.png" alt="картинка.">
        <div class="description__img-blur" />
        <div class="stub" :style="{width: '190px', height: '26px', marginBottom: '24px', borderRadius: '8px'}" />
        <div class="stub" :style="{width: '155px'}" />
        <div class="stub" :style="{width: '190px'}" />
        <div class="stub" :style="{width: '170px'}" />
        <div class="stub" :style="{width: '160px'}" />
        <div class="stub" :style="{width: '140px'}" />
        <div class="stub" :style="{width: '80px', margin: '8px auto 0'}" />
      </div>

      <ul class="section grid">
        <li class="grid__item" v-for="i in 25" :key="i" @click="handleItemClick($event, i)" @drop="onDrop($event, i)"
          @dragover.prevent @dragenter.prevent>
          <div class="item" v-if="findItem(i)">
            <div class="item__img-container" :draggable="true" @dragstart="onDragStart($event, findItem(i))">
              <div class="item__back-one" :style="{background: findItem(i).colors[0]}" />
              <div class="item__back-two" :style="{background: findItem(i).colors[1]}" />
            </div>
            <span class="item__counter">{{findItem(i).amount}}</span>
          </div>
        </li>
      </ul>


      <Transition>
        <inventory-popup :show="isPopupOpen" :isDeleteOpen="isDeletePopupOpen" :element="currentElement"
          @hide="closePopup" @delete="handleDelete" @openDeleteModal="openDeleteModal"
          @closeDeleteModal="closeDeleteModal">
          <div class="stub" :style="{width: '211px', height: '30px', marginBottom: '24px', borderRadius: '8px'}" />
          <div class="stub" :style="{width: '211px'}" />
          <div class="stub" :style="{width: '211px'}" />
          <div class="stub" :style="{width: '211px'}" />
          <div class="stub" :style="{width: '180px'}" />
          <div class="stub" :style="{width: '80px', margin: '0'}" />
        </inventory-popup>
      </Transition>

    </div>

    <div class="section footer">
      <div class="stub stub__footer" />
      <button type="button" class="footer__close-btn" />
    </div>
  </section>
</template>

<script>
  import items from "../assets/items.json";
  import InventoryPopup from "./InventoryPopup.vue";

  export default {
    name: 'App',
    components: { InventoryPopup },
    data() {
      return {
        itemsList: items,
        isPopupOpen: false,
        isDeletePopupOpen: false,
        currentElement: {}
      }
    },
    methods: {
      handleItemClick(evt, id) {
        const el = evt.currentTarget.querySelector('.item');

        if (el !== null) {
          if (this.isPopupOpen) {
            this.closePopup();
            setTimeout(() => {
              this.openPopup();
            }, 500)
          } else {
            this.openPopup();
          }
          this.currentElement = this.itemsList.find((i) => i.location === id);
        } else {
          this.closePopup();
        }
      },
      findItem(evt) {
        return this.itemsList.find((i) => i.location === evt)
      },
      openPopup() {
        this.isPopupOpen = true;
      },
      closePopup() {
        this.isPopupOpen = false;
        this.closeDeleteModal();
      },
      openDeleteModal() {
        this.isDeletePopupOpen = true;
      },
      closeDeleteModal() {
        this.isDeletePopupOpen = false;
      },
      handleDelete(num) {
        const item = this.itemsList.find((e) => e.id === this.currentElement.id);
        if (item.amount <= num) {
          this.itemsList.splice(this.itemsList.indexOf(item), 1);
        } else {
          item.amount -= num;
        }
      },
      onDragStart(evt, item) {
        evt.dataTransfer.setData('itemLocation', JSON.stringify(item.location));

        if (this.isPopupOpen) {
          this.closePopup();
        }
      },
      onDrop(evt, location) {
        let itemLocation = JSON.parse(evt.dataTransfer.getData('itemLocation'));
        const itemErr = this.itemsList.find((e) => e.location === location);
        if (itemErr) {
          alert('Этот слот уже занят!')
        } else {
          this.itemsList.map(i => {
            if (i.location === itemLocation) {
              i.location = location;
            }
          })
        }
      }
    }
  }
</script>

<style>
  @font-face {
    font-family: 'SF Pro Display';
    src: url("../assets/fonts/SFProDisplay-Regular.woff") format("woff"),
      url("../assets/fonts/SFProDisplay-Regular.woff2") format("woff2");
  }

  * {
    font-family: 'SF Pro Display';
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .app {
    width: 100%;
    height: 100vh;
    background-color: #1E1E1E;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .app__main-container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-bottom: 24px;
    position: relative;
    overflow: hidden;
  }

  .section {
    background-color: #262626;
    border: 1px solid #4D4D4D;
    border-radius: 12px;
    overflow: hidden;
  }

  .description {
    width: 236px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 18px 14px 24px;
    position: relative;
    margin-right: 24px;
  }

  .description__img {
    width: 100%;
    height: 240px;
    margin-bottom: 20px;
    border-radius: 8px;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(6px);
  }

  .description__img-blur {
    position: absolute;
    top: 18px;
    right: 14px;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(6px);
    border-radius: 8px;
    z-index: 2;
    width: 208px;
    height: 240px;
  }

  .stub {
    background: linear-gradient(90deg, #3C3C3C 0%, #444444 51.04%, #333333 100%);
    border-radius: 4px;
    margin-bottom: 16px;
    height: 10px;
  }

  .grid {
    display: grid;
    background-color: #4D4D4D;
    grid-gap: 1px;
    grid-template-columns: repeat(5, 105px);
    grid-template-rows: repeat(5, 100px);
    list-style-type: none;
  }

  .grid__item {
    background-color: #262626;
  }

  .item {
    position: relative;
    width: 105px;
    height: 100px;
  }

  .item__img-container {
    width: 100%;
    height: 100%;
  }

  .item__back-one {
    width: 48px;
    height: 48px;
    position: absolute;
    bottom: 23px;
    left: 26px;
  }

  .item__back-two {
    width: 48px;
    height: 48px;
    backdrop-filter: blur(6px);
    position: absolute;
    top: 23px;
    right: 25px;
    z-index: 3;
  }

  .item__counter {
    border: 1px solid #4D4D4D;
    border-right: none;
    border-bottom: none;
    border-radius: 6px 0px 0px 0px;
    font-style: normal;
    font-weight: 500;
    font-size: 10px;
    line-height: 12px;
    padding: 2px 4px;
    color: rgba(255, 255, 255, 0.4);
    position: absolute;
    bottom: 0;
    right: 0;
  }

  .footer {
    width: 100%;
    max-width: 785px;
    padding: 18px;
    position: relative;
  }

  .stub__footer {
    width: 699px;
    height: 36px;
    border-radius: 12px;
    margin: 0 auto 0 0;
  }

  .footer__close-btn {
    background-image: url("../assets/images/close-btn.svg");
    background-size: cover;
    background-color: #262626;
    width: 12px;
    height: 12px;
    position: absolute;
    top: 14px;
    right: 14px;
    border: none;
    transition: all 0.3s ease-in-out;
  }

  .footer__close-btn:hover {
    opacity: 0.7;
    cursor: pointer;
    transition: all 0.3s ease-in-out;
  }

  .v-enter-active,
  .v-leave-active {
    transition: transform 0.5s ease;
  }

  .v-enter-from,
  .v-leave-to {
    transform: translateX(250px);
    transition: transform 0.5s ease;
  }
</style>