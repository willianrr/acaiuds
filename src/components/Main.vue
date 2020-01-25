<template>
  <div class="main">
    <div class="wrapper">
      <div class="initial" v-if="current_menu == 'initial'">
        <div class="header">
          <h3>Bem-Vindo à AÇAI UDS</h3>
        </div>
        <div class="logo">
          <img src="../assets/img/logo-acai.png" alt="logo uds"/>
        </div>
        <div class="body">
          <button @click="change_menu('pedido')">Escolher pedido</button>
        </div>
      </div>
      <div class="initial" v-if="current_menu == 'pedido'">
        <div class="header">
          <h3>Faça seu pedido</h3>
        </div>
        <div class="body-demanded">
          <div class="pictures">
            <div class="item"
                 v-for="acai in acais"
                 :key="acai.id"
                 @click="pick_acai(acai)">
              <img src="@/assets/img/acai.png" alt="açai"/>
              <h3>{{acai.name}}</h3>
              <h6>Preço R${{acai.price}}</h6>
            </div>
          </div>
          <div class="option-body">
            <div class="flavor">
              <ul>
                <li v-for="flavor in flavors" :key="flavor.id" @click="pick_flavor(flavor)">
                  {{flavor.name}}
                </li>
              </ul>
              <div class="flavor-choice" v-show="flavorShow">Escolha um Açai</div>
            </div>
            <div class="customize">
              <ul>
                <li v-for="customize in customizes" :key="customize.id" @click="pick_custom(customize)">
                  {{customize.name}}
                </li>
              </ul>
              <div class="customize-choice" v-show="customizeShow">Escolha um Sabor.</div>
            </div>
          </div>
          <div class="history">
            <table class="table">
              <thead>
              <tr>
                <th scope="col">Tamanho</th>
                <th scope="col">Sabor</th>
                <th scope="col">Personalizações</th>
                <th scope="col">Tempo de preparo</th>
                <th scope="col">Valor total</th>
              </tr>
              </thead>
              <tbody>
              <tr>
                <td>{{chooseAcai || 'Escolher'}}</td>
                <td>{{chooseFlavor || 'Escolher'}}</td>
                <td>{{chooseCustom || 'Escolher'}}</td>
                <td>{{totalTime || '0'}}min</td>
                <td>R$: {{totalPrice || '0'}}</td>
              </tr>
              </tbody>
            </table>
          </div>
          <div class="button-send">
            <button type="button" :disabled="buttonSend" @click="change_menu('success')">Fazer pedido</button>
          </div>
        </div>
      </div>
      <div class="initial" v-if="current_menu == 'success'">
        <div class="body-success">
          <div class="button-back">
            <button class="back-initial" @click="reloadPage">Voltar</button>
          </div>
          <h2>Pedido Feito com sucesso!!!</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    export default {
        name: 'HelloWorld',

        data() {
            return {
                current_menu: 'initial',
                chooseAcai: '',
                chooseFlavor: '',
                chooseCustom: '',
                acais: [
                    {
                        id: 0,
                        name: 'Açai pequeno (300ml)',
                        price: 10,
                        time: 5,
                        size: 'Pequeno'
                    },
                    {
                        id: 1,
                        name: 'Açai médio (500ml)',
                        price: 13,
                        time: 7,
                        size: 'Médio'
                    },
                    {
                        id: 2,
                        name: 'Açai grande (700ml)',
                        price: 15,
                        time: 10,
                        size: 'Grande'
                    }
                ],
                flavors: [
                    {
                        id: 0,
                        name: 'Morango',
                        price: 0,
                        time: 0
                    },
                    {
                        id: 1,
                        name: 'Banana',
                        price: 0,
                        time: 0
                    },
                    {
                        id: 2,
                        name: 'Kiwi',
                        price: 0,
                        time: 5
                    }
                ],
                customizes: [
                    {
                        id: 0,
                        name: 'Granola',
                        price: 0,
                        time: 0
                    },
                    {
                        id: 1,
                        name: 'Paçoca',
                        price: 3,
                        time: 3
                    },
                    {
                        id: 2,
                        name: 'Leite ninho',
                        price: 3,
                        time: 0
                    }
                ],
                totalPrice: 0,
                totalTime: 0,
                flavorShow: true,
                customizeShow: true,
                flavorLimit: 0,
                buttonSend: true
            }

        },
        methods: {
            change_menu: function (new_menu) {
                this.current_menu = new_menu;
            },
            pick_acai(acai) {
                this.chooseAcai = acai.size;
                this.flavorShow = false;
                if (acai.size.length) {
                    this.totalPrice = acai.price;
                    this.totalTime = acai.time;
                }
            },
            pick_flavor(flavor) {
                this.chooseFlavor = flavor.name;
                this.customizeShow = false;
                this.buttonSend = false;
                if (this.flavorLimit < 1) {
                    return this.totalTime;
                } else {
                    this.totalTime = this.totalTime + flavor.time;
                }
            },
            pick_custom(customize) {
                this.chooseCustom = customize.name;
                if (customize.name.length) {
                    this.totalPrice = this.totalPrice + customize.price;
                    this.totalTime = this.totalTime + customize.time;
                }
            },
            reloadPage() {
                window.location.reload();
            }
        },
    }

</script>

<style lang="scss" scoped>
  .main {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;

    .wrapper {
      border: 2px solid #552555;
      width: 800px;
      height: 700px;
      background-color: #552555;
      border-radius: 2px;

      .initial {
        width: 100%;
        height: 100%;
        position: relative;

        .header {
          text-align: center;
          color: #222;
          width: 100%;
          background-color: #fff;

          h3 {
            padding: 10px 0 10px 0;
            margin: 0;
            color: #552555;
          }
        }

        .logo {
          img {
            margin: auto;
            display: block;
          }
        }

        .body {
          width: 100%;
          display: flex;
          justify-content: center;

          button {
            background-color: #ffffff;
            color: #552555;
            border: 1px solid #fff;
            padding: 14px 24px;
            border-radius: 4px;
            font-size: 20px;
            margin-top: 2rem;
            font-weight: 600;

            &:hover {
              cursor: pointer;
              background-color: #552555;
              color: #ffffff;
            }
          }
        }

        .body-demanded {
          margin: 1rem;
          background-color: #fff;

          .pictures {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;

            .item {
              background-color: #fff;
              margin: 2px;
              border: 2px solid #552555;
              color: #552555;

              &:hover {
                background-color: #552555a3;
                cursor: pointer;
                border: 2px solid #222;
                color: #fff;
              }

              img {
                margin: auto;
                display: block;
              }

              h3 {
                text-align: center;
              }

              h6 {
                margin: 4px;
              }
            }
          }

          .option-body {
            display: grid;
            grid-template-columns: 1fr 1fr;

            .flavor {
              background-color: #fff;
              margin: 2px;
              border: 2px solid #552555;
              color: #552555;
              position: relative;
              display: flex;

              ul {
                padding: 0 5px 0 5px;
                width: 100%;

                li {
                  padding: 5px;
                  margin: 3px;

                  &:hover {
                    cursor: pointer;
                    background-color: #552555;
                    color: #ffffff;
                  }
                }
              }

              .flavor-choice {
                position: absolute;
                justify-content: center;
                align-items: center;
                display: flex;
                width: 100%;
                height: 100%;
                color: #fff;
                background-color: #552555b3;
              }
            }

            .customize {
              background-color: #fff;
              margin: 2px;
              border: 2px solid #552555;
              color: #552555;
              position: relative;
              display: flex;

              ul {
                padding: 0 5px 0 5px;
                width: 100%;

                li {
                  padding: 5px;
                  margin: 3px;

                  &:hover {
                    cursor: pointer;
                    background-color: #552555;
                    color: #ffffff;
                  }
                }
              }

              .customize-choice {
                position: absolute;
                justify-content: center;
                align-items: center;
                display: flex;
                width: 100%;
                height: 100%;
                color: #fff;
                background-color: #552555b3;
              }
            }
          }
        }

        .history {
          width: 100%;
          height: 100%;
          border: 2px solid #fff;

          table {
            margin-bottom: 0;

            thead {
              background-color: #552555;
              color: #fff;
            }

            tr {
              border: 2px solid #552555;
              text-align: center;
            }

            th {
              border-bottom: none;
            }

            td {
              text-align: center;
            }
          }
        }

        .button-send {
          position: absolute;
          bottom: 1rem;
          right: 1rem;

          button {
            background-color: #ffffff;
            color: #552555;
            border: 1px solid #fff;
            padding: 7px 12px;
            border-radius: 4px;
            font-size: 20px;
            font-weight: 500;

            &:hover {
              background-color: #552555;
              color: #ffffff;
              cursor: pointer;
            }
          }
        }

        .body-success {
          align-items: center;
          justify-content: center;
          width: 100%;
          height: 100%;

          h2 {
            color: #fff;
            justify-content: center;
            align-items: center;
            display: flex;
            height: 100%;
            text-align: center;
          }

          .button-back {
            position: absolute;
            top: 1rem;
            right: 1rem;

            .back-initial {
              background-color: #ffffff;
              color: #552555;
              border: 1px solid #fff;
              padding: 7px 12px;
              border-radius: 4px;
              font-size: 20px;
              font-weight: 500;

              &:hover {
                background-color: #552555;
                color: #ffffff;
                cursor: pointer;
              }
            }
          }
        }
      }
    }
  }

  @media screen and (max-width: 768px) {
    .main {
      .wrapper {
        width: 100%;
        height: 100%;
      }
    }
  }
  @media screen and (max-width: 500px) {
    .main {
      .wrapper {
        .initial {
          .body-demanded {
            margin: unset;
            .pictures {
              grid-template-columns: auto;
            }
            .option-body {
              grid-template-columns: auto;
            }
          }
          .logo {
            img {
              width: 100%;
            }
          }
          .history {
            table {
              display: none;
            }
          }
          .button-send {
            position: unset;
            button {
              border: 2px solid #552555;
            }
          }
        }
      }
    }
  }
</style>
