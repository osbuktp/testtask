<template>
  <div class="product">
    <div class="product_photo">
      <img :src="product.primaryImageUrl.slice(0, -4) + '_220x220_1' + product.primaryImageUrl.slice(-4)" alt="Изображение товара">
    </div>
    <div class="product_info">
      <div class="product_meta">
        <div class="product_code">Код: {{parseInt(product.code)}}</div>
        <div class="product_status">Наличие</div>
      </div>
      <div class="product_title">{{product.title}}</div>
      <div class="product_associations">
        <b>Могут понадобиться:</b>
        <a
          v-for="(assocProduct, id) in product.assocProducts.split(';').slice(0, -1)"
          :key="id"
          href="#"
          class="assoc-link"
        >{{assocProduct}}</a>
      </div>
    </div>
    <div class="product_purchase">
      <div class="price">
        <div class="price__gold">
          По карте клуба:
          <span class="price__big">
            {{altPrice ? product.priceGold : product.priceGoldAlt | roundPrice}}
            <svg
              version="1.0"
              id="rouble__b"
              xmlns="http://www.w3.org/2000/svg"
              x="0"
              y="0"
              width="24px"
              height="18px"
              viewBox="0 0 50 50"
              enable-background="new 0 0 50 50"
              xml:space="preserve"
            >
              <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_black"></use>
            </svg>
          </span>
        </div>
        <div class="price__retail">
          <span class="price__big">
            {{altPrice ? product.priceRetail: product.priceRetailAlt | roundPrice}}
            <svg
              version="1.0"
              id="rouble__g"
              xmlns="http://www.w3.org/2000/svg"
              x="0"
              y="0"
              width="24px"
              height="18px"
              viewBox="0 0 50 50"
              enable-background="new 0 0 50 50"
              xml:space="preserve"
            >
              <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#rouble_gray"></use>
            </svg>
          </span>
        </div>
        <div class="price__points">Можно купить за {{"333,33"}} балла</div>
        <div class="price_format">
          <div @click="altPrice=false" :class="{'active': !altPrice}" class="format-option">За м. кв.</div>
          <div @click="altPrice=true" :class="{'active': altPrice}" class="format-option">За упаковку</div>
        </div>
      </div>
      <div class="purchase_options">
        <div class="purchase_tooltip">
          <div>Продается упаковками:
            <br>1 упак. = {{product.priceRetail / product.priceRetailAlt | roundPrice}} м. кв.
          </div>
        </div>
        <div class="counter">
          <input @input="checkNumeric($event.target.value)" :value="count" type="text" class="counter_input">
          <div class="counter_buttons">
            <div @click="incCounter" class="counter_button_inc"></div>
            <div @click="decCounter" class="counter_button_dec"></div>
          </div>
        </div>
        <div :data-product-id="product.code" class="purchase_button">
          <span class="purchase_button_text">
            <svg class="ic ic_cart">
              <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#cart"></use>
            </svg>
            <span >В корзину</span>
          </span>
        </div>
      </div>
    </div>
    <svg viewBox="0 0 497 415" xmlns="http://www.w3.org/2000/svg" display="none">
      <symbol id="rouble_black">
        <path
          fill="#010101"
          d="M44.431,6.112c-1.719-2.395-4.211-4.125-7.055-4.899c-3.884-0.647-7.824-0.894-11.759-0.735H9.743v22.194H3.031v8.329h6.712v4.556H3.031v8.329h6.712v5.683h9.799v-5.634h14.698v-8.329H19.542v-4.507h6.467c3.427,0.083,6.854-0.147,10.24-0.686c1.764-0.451,3.427-1.232,4.899-2.303c1.763-1.273,3.192-2.954,4.165-4.899c1.155-2.36,1.711-4.968,1.617-7.594C47.17,12.259,46.291,8.917,44.431,6.112z M35.955,19.585c-0.757,1.1-1.835,1.938-3.087,2.401c-2.557,0.664-5.201,0.928-7.839,0.784h-5.487V8.855h4.899c2.373-0.064,4.748,0.051,7.104,0.343c1.528,0.25,2.927,1.01,3.969,2.156c1.068,1.215,1.629,2.793,1.568,4.409C37.112,17.123,36.718,18.459,35.955,19.585L35.955,19.585z"
        ></path>
      </symbol>
      <symbol id="rouble_gray">
        <path
          fill="#a7a7a7 "
          d="M28.109,29.658c5.173,0.578,10.357-0.979,14.355-4.312c4.172-4.748,5.151-11.509,2.499-17.246c-0.847-1.953-2.2-3.644-3.92-4.899c-1.813-1.204-3.872-1.991-6.026-2.303c-2.464-0.37-4.955-0.534-7.447-0.49H9.05v23.272H3.562v5.781H9.05v5.977H3.562v5.781H9.05v8.378h6.467v-8.133h20.088v-5.781H15.517v-6.026H28.06H28.109z M15.566,6.386h12.543c2.019-0.074,4.04,0.074,6.026,0.441c1.673,0.442,3.132,1.467,4.116,2.891c1.062,1.539,1.611,3.373,1.568,5.242c0.138,2.473-0.832,4.879-2.646,6.565c-2.601,1.812-5.76,2.645-8.917,2.352H15.517V6.386H15.566z"
        ></path>
      </symbol>
      <symbol id="cart">
        <path
          d="m14.571 16.381c.571 0 .952.381.952.952 0 .571-.381.952-.952.952-.571 0-.952-.381-.952-.952 0-.571.476-.952.952-.952m0-.952c-1.048 0-1.905.857-1.905 1.905 0 1.048.857 1.905 1.905 1.905 1.048 0 1.905-.857 1.905-1.905 0-1.048-.857-1.905-1.905-1.905"
        ></path>
        <path
          d="m7.905 16.381c.571 0 .952.381.952.952 0 .571-.381.952-.952.952-.571 0-.952-.381-.952-.952 0-.571.476-.952.952-.952m0-.952c-1.048 0-1.905.857-1.905 1.905 0 1.048.857 1.905 1.905 1.905 1.048 0 1.905-.857 1.905-1.905 0-1.048-.857-1.905-1.905-1.905"
        ></path>
        <path
          d="m16.476 14.476h-10.857l-.095-.381c0-.095-1.429-9.714-1.905-11.524-.381-1.524-3.333-1.429-3.333-1.429v-.952c.095 0 3.714-.286 4.286 2.19.381 1.714 1.619 9.333 1.81 11.143h10.1v.952"
        ></path>
        <path d="m4.095 3.048h15.238v.952h-15.238z"></path>
        <path d="m5.05 10.667h12.381v.952h-12.381z"></path>
        <path d="m16.476 11.619h.952l1.905-8.571h-.952l-1.905 8.571"></path>
      </symbol>
    </svg>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: ["product"],
  filters: {
    roundPrice(price) {
      return Number(price.toFixed(2));
    }
  },
  data() {
    return {
      count: 1,
      altPrice: false
    }
  },
  methods: {
    incCounter() {
      this.count++
    },
    decCounter() {
      if (this.count > 0) this.count--;
    },
    checkNumeric(value) {
      if (Number.isNaN(Number.parseInt(value))) {
        this.count = 0
      } else {
        this.count = Number.parseInt(value)
      };
    }
  }
};
</script>

<style scoped lang="scss">
.product {
  box-sizing: border-box;
  padding: 10px 10px 18px;
  border: 1px solid #a7a7a7;
  display: inline-block;

  &_photo,
  &_info,
  &_purchase {
    float: left;
    box-sizing: border-box;
  }

  &_photo {
    margin-left: auto;
    margin-right: auto;
    width: 160px;
    height: 160px;
    overflow: hidden;
    img {
      height: 100%;
    }
  }

  &_info {
    width: 400px;
    padding: 0 5px;
    text-align: left;
    .product {
      &_meta {
        margin-bottom: 18px;
      }
      &_code,
      &_status {
        display: inline;
        font-size: 12px;
      }
      &_code {
        margin-right: 14px;
      }
      &_status {
        cursor: pointer;
        border-bottom: 1px dotted green;
        color: green;
      }

      &_title {
        font-size: 14px;
        font-weight: bold;
        margin-bottom: 18px;
      }

      &_associations {
        font-size: 13px;
        color: #666;
        .assoc-link {
          font-size: 12px;
          color: inherit;
          line-height: 14px;
          text-decoration: none;
          &::after {
            content: ",";
          }
          &:last-child::after {
            content: "";
          }
        }
      }
    }
  }

  &_purchase {
    width: 240px;
    float: right;
    .price {
      text-align: right;
      &__retail {
        color: #a7a7a7;
        margin-bottom: 6px;
      }
      &__big {
        font-size: 25px;
      }
      &__points {
        padding-right: 5px;
        color: #a7a7a7;
        font-size: 12px;
        margin-bottom: 6px;
      }
      &_format {
        margin-bottom: 6px;
        font-size: 13px;
        .format-option {
          cursor: pointer;
          display: inline-block;
          color: #a7a7a7;
          margin-right: 5px;
          padding: 3px 5px;
          border-bottom: 1px dotted #a7a7a7;
        }
        .active {
          color: white;
          background-color: black;
          border: none;
        }
      }
    }
    .purchase {
      &_options {
        .purchase {
          &_tooltip {
            div {
              position: relative;
              padding-left: 38px;
              &::before {
                content: "";
                position: absolute;
                width: 32px;
                left: 0;
                top: 0;
                bottom: 0;
                background-size: 16px 16px;
                background-repeat: no-repeat;
                background-position: center;
                background-image: url("../assets/img/tooltip.png");
              }
            }
            font-size: 12px;
            text-align: left;
            padding: 3px 5px;
            border: 1px solid #ccc;
            position: relative;
            margin-bottom: 14px;
            &::before,
            &::after {
              content: "";
              position: absolute;
              bottom: -8px;
              left: 13px;
              border-top: 8px solid #ccc;
              border-left: 8px solid transparent;
              border-right: 8px solid transparent;
              width: 0;
              height: 0;
            }
            &::after {
              margin-bottom: 1.5px;
              border-top: 8px solid white;
            }
          }
        }
      }
    }
    .counter {
      float: left;
      &_input {
        float: left;
        width: 46px;
        height: 38px;
        text-align: center;
        font-size: 16px;
        box-sizing: border-box;
        padding: 4px;
        border: 1px solid #a7a7a7;
      }
      &_buttons {
        float: left;
        width: 19px;
        height: 34px;
        .counter_button {
          &_inc,
          &_dec {
            height: 17px;
            cursor: pointer;
            border: 1px solid #a7a7a7;
            border-left: 0px;
            background-position-x: 23px;
            background-image: url("../assets/img/count.png");
            &:hover {
              background-color: #a7a7a7;
              background-position-x: -4px;
            }
          }
          &_inc {
            height: 18px;
            background-position-y: -2px;
            border-bottom: 0px;
          }
          &_dec {
            background-position-y: -22px;
          }
        }
      }
    }

    .purchase {
      &_button {
        height: 38px;
        float: left;
        margin-left: 5px;
        background-color: #f90;
        width: calc(100% - 72px);
        position: relative;
        cursor: pointer;
        svg {
          overflow: hidden;
          width: 20px;
          height: 20px;
          position: absolute;
          left: 20px;
          top: 8px;
          fill: #FFF;
        }
        &_text {
          line-height: 38px;
          margin-left: 20px;
          color: white;
          text-transform: uppercase;
        }
        &:hover {
          background-color: black;
        }
      }
    }
  }
}
</style>
