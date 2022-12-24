<template>
    <article class="popup" v-if="show">
        <button type="button" class="popup__close-btn" @click.stop="$emit('hide')" />

        <div class="popup__item-one" :style="{background: element.colors[0]}" />
        <div class="popup__item-two" :style="{background: element.colors[1]}" />

        <div class="popup__line" />

        <slot></slot>

        <div class="popup__line popup__line_down" />

        <button type="button" class="popup__delete-btn" @click.stop="openDeleteModal">Удалить предмет</button>

        <form class="popup-delete" v-if="isDeleteOpen" @submit.prevent>
            <input class="popup-delete__input" v-model="elCount" min=1 max="element.amount" type="number" />
            <div class="popup-delete__btns">
                <button type="button" class="popup-delete__btn-cancel" @click="closeDeleteModal">Отмена</button>
                <button type="submit" class="popup-delete__btn-submit" @click="onDelete">Подтвердить</button>
            </div>
        </form>
    </article>
</template>

<script>
    export default {
        props: {
            element: Object,
            show: Boolean,
            isDeleteOpen: Boolean,
        },
        data() {
            return {
                elCount: 1
            }
        },
        methods: {
            openDeleteModal() {
                this.$emit('openDeleteModal');
                this.elCount = 1;
            },
            closeDeleteModal() {
                this.$emit('closeDeleteModal');
            },
            onDelete() {
                if (this.elCount > 0) {
                    this.closeDeleteModal();
                    this.$emit('delete', this.elCount);
                    this.$emit('hide');
                }
            }
        }
    }
</script>

<style scoped>
    .popup {
        position: absolute;
        top: 0;
        right: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 250px;
        height: 100%;
        background: rgba(38, 38, 38, 0.95);
        border: 1px solid #4D4D4D;
        border-radius: 0 12px 12px 0;
        z-index: 3;
    }

    .popup__close-btn {
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

    .popup__close-btn:hover {
        opacity: 0.7;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
    }

    .popup__item-one {
        position: absolute;
        width: 115.56px;
        height: 115.56px;
        top: 69px;
        right: 74px;
    }

    .popup__item-two {
        width: 115.56px;
        height: 115.56px;
        position: absolute;
        top: 55px;
        right: 60px;
        backdrop-filter: blur(6px);
    }

    .popup__line {
        width: 220px;
        height: 1px;
        background: #4D4D4D;
        margin: 215px auto 16px;
    }

    .popup__line_down {
        margin: 24px auto 18px;
    }

    .popup__delete-btn {
        width: 220px;
        height: 39px;
        background: #FA7272;
        border-radius: 8px;
        border: none;
        transition: all 0.3s ease-in-out;
        margin: 0 auto 18px;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 17px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #FFFFFF;
    }

    .popup__delete-btn:hover {
        background: #FF8888;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
    }

    .popup-delete {
        position: absolute;
        bottom: 0;
        right: 0;
        width: 100%;
        background: rgba(38, 38, 38, 0.6);
        border-top: 1px solid #4D4D4D;
        backdrop-filter: blur(8px);
        border-radius: 0 0 12px 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px 21px;
    }

    .popup-delete__input {
        width: 210px;
        height: 40px;
        background: #262626;
        border: 1px solid #4D4D4D;
        border-radius: 4px;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 17px;
        color: #FFFFFF;
        padding: 0 7px;
    }

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
    }

    .popup-delete__input:focus-visible {
        outline: none;
    }

    .popup-delete__btns {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-top: 20px;
    }

    .popup-delete__btn-cancel,
    .popup-delete__btn-submit {
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        border-radius: 8px;
        flex: none;
        order: 0;
        flex-grow: 0;
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 17px;
        border: none;
        height: 33px;
        cursor: pointer;
    }

    .popup-delete__btn-cancel {
        background: #FFFFFF;
        color: #2D2D2D;
        width: 88px;
        margin-right: 10px;
    }

    .popup-delete__btn-submit {
        background: #FA7272;
        color: #FFFFFF;
        width: 112px;
    }
</style>