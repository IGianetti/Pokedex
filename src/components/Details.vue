<template>
  <div class="detail">
    <div class="detail-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="" />
      </div>
      <div v-if="pokemon" class="data">
        <h2>{{ pokemon.name }}</h2>

        <div class="property">
          <div class="prop1">
            <h3>Types</h3>
            <div
              class="type"
              v-for="(value, index) in pokemon.types"
              :key="'value' + index"
            >
              <button class="btn-type">{{ value.type.name }}</button>
            </div>
          </div>
          <div class="prop2">
            <div class="height">
              <h3>Height</h3>
              <p>{{ pokemon.height / 10 }} m</p>
            </div>
          </div>

          <div class="prop3">
            <div class="weight">
              <h3>Weight</h3>
              <p>{{ pokemon.weight / 10 }} kg</p>
            </div>
          </div>
        </div>

        <h3>Base Stats</h3>
        <div class="abilities">
          <div
            class="ability"
            v-for="(value, index) in pokemon.stats"
            :key="'value' + index"
          >
            <div class="left">
              {{ value.stat.name }}
              <progress
                class="barrita"
                max="255"
                :value="value.base_stat"
              ></progress>
            </div>

            <div class="right">{{ value.base_stat }}</div>
          </div>
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
      <button class="close" @click="closeDetail">close</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl"],

  data: () => {
    return {
      show: false,
      pokemon: {},
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.pokemon = data;
          this.show = true;
          console.log(data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    closeDetail() {
      this.$emit("closeDetail");
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: calc(100% - 20px);
  height: calc(100vh - 20px);
  background: rgba($color: #000000, $alpha: 0.7);
  .detail-view {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: relative;
    width: 100%;
    max-width: 510px;
    padding: 50px 0 0;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);

    .image {
      display: flex;
      justify-content: center;
      align-items: center;
      position: absolute;
      top: -60px;
      width: 120px;
      height: 120px;
      background-color: #333;
      border-radius: 50%;
      overflow: hidden;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
    }
    h2 {
      text-transform: capitalize;
    }
    .data {
      display: flex;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin-bottom: 40px;

      .property {
        display: flex;
        justify-content: center;
        width: 100%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
        margin-bottom: 10px;

        .prop1 {
          width: 136px;

          .type {
            width: 50%;
            display: table-cell;

            .btn-type {
              outline: none;
              border: none;
              font-size: 1rem;
              font-size: 1rem;
              background-color: initial;
            }
          }
        }
        .prop2 {
          width: 130px;
          .height p {
            margin-left: 40px;
          }
        }
        .prop3 {
          width: 130px;
          .weight p {
            margin-left: 40px;
          }
        }

        h3 {
          width: 90%;
          max-width: 400px;
          border-bottom: 1px solid #ccc;
        }
      }
      .abilities {
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 80%;

        .ability {
          margin-top: 3px;
          padding: 10px 10px;
          color: rgb(33, 33, 33);
          font-size: 1rem;
          letter-spacing: 2px;
          text-transform: capitalize;
          word-wrap: none;
          word-break: keep-all;
          border-bottom: 1px solid #ccc;

          .left {
            float: left;
            .barrita {
              border-radius: 10px;
              width: 200px;
              height: 10px;
              margin-left: 15px;
            }
            .barrita::-webkit-progress-bar {
              background: rgb(194, 194, 194);
              border-radius: 10px;
            }
            .barrita::-webkit-progress-value {
              background: chocolate;
              border-radius: 10px;
            }
          }
          .right {
            float: right;
          }
        }
      }
    }
    .close {
      outline: none;
      border: none;
      border-radius: 5px;
      background-color: #333;
      color: #efefef;
      padding: 10px 20px;
      margin-bottom: 20px;
      font-size: 1.2rem;
      cursor: pointer;
    }
  }
  i {
    font-size: 2rem;
    color: #efefef;
  }
}
</style>
