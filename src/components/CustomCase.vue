<template lang="html">
  <div>
        <div class='col-md-12 title'>Admin - CustomCase</div>

    <div class='col-md-2' style='font-size:15px; padding-top:5px'>Filter:</div>
    <div class='col-md-5' style='margin-bottom:15px'><select class=' form-control' v-model='orderPodaci.filter' v-on:change='reset'><option v-bind:value='1'>Neobradjene</option><option v-bind:value='2'>Poslate</option><option v-bind:value='3'>Odbijene</option></select></div>
    <div class='col-md-5' style='padding-top:5px'>Ukupno porudzbina: {{orderBazaPodaci.length}}</div>


          <div class='col-md-3 pull-left'>
          <table class='table table-bordered table-hover'><tbody>
          <tr>
            <th>ID</th>
            <th>Status</th>
            <th>Datum</th>
            <th><i class='fa fa-search'></i></th>
          </tr>
          <tr v-for='ord in orderBazaPodaci' v-if='ord.status.id == orderPodaci.filter'  >
            <td>{{ ord.id }}</td>
            <td>{{ord.status.id == 1 ? 'N' : ord.status.id == 2 ? 'P' : 'O' }}</td>
            <td>{{ ord.created_at }}</td>
            <td><button @click='openPanel(ord.id)' class='btn btn-primary btn-xs'>Pogledaj</button></td>
          </tr>
          </tbody>
        </table>
      </div>
          <div class='col-md-9 pull-left' v-if='orderPodaci.clicked'>
          <table class='table table-bordered'><tbody>
          <tr>
          <td>ID</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.id}}</td>
          </tr>
          <tr>
          <td>Ime i prezime</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.user.firstName+" "+orderPodaci.user.lastName}}</td>
          </tr>
          <tr>
          <td>Slika</td>
          <td v-if='orderPodaci.clicked'>
            <td v-if='orderPodaci.clicked'><img :src='orderPodaci.picture' width='100'/></td>
        </td>
          </tr>
          <tr>
          <td>Cena</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.amount*1200}}</td>
          </tr>
          <tr>
          <td>Status</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.status.id == 1 ? 'Nije obradjena' : orderPodaci.status.id == 2 ? 'Poslata' : 'Odbijena' }}</td>
          </tr>
          <tr>
          <td>Datum</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.created_at}}</td>
          </tr>
          <tr>
          <td>Adresa</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.user.address}}</td>
          </tr>
          <tr>
          <td>Telefon</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.user.phone}}</td>
          </tr>
          <tr>
          <td>Grad</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.user.city}}</td>
          </tr>
          <tr>
          <td>Poštanski broj</td>
          <td v-if='orderPodaci.clicked'>{{orderPodaci.user.postNumber}}</td>
          </tr>
          <tr>
          <td colspan='8' v-if='orderPodaci.filter == 1'><button class='btn btn-success' @click='posalji(2)'>Potvrdi</button>&nbsp;<button class='btn btn-danger' @click='posalji(3)'>Otkazi</button></td>
          </tr>
          </tbody></table>
          </div>
          <div class='col-md-9 pull-left' v-else></div>
          <div id='feedback' class='alert alert-success nev'></div>
          <div id='err' class='alert alert-danger nev'></div>

          </div>

</template>
<script>
    // import '../js/ajax.js'
    export default {
        data: function() {
            return {
                orderBazaPodaci: orderDbData.cases,
                orderPodaci: orderData
            }
        },
        methods: {
            reset: function() {
                this.orderPodaci.clicked = false;
            },
            posalji: function(i) {
                this.resetHolders();
                switch (i) {
                    case 2:
                        var status_id = 2;
                        break;
                    case 3:
                        var status_id = 3;
                        break;
                    default:
                }
                var data = {
                    statusId : status_id
                }
                console.log(data);
                $.ajax({
                    url: window.base_url+'/custom-case/'+this.orderPodaci.id,
                    type: 'PATCH',
                    dataType: "json",
                    data: data,
                    success: function(data) {
                        $('#feedback').html('Uspešno izvršeno!');
                    },
                    error: function(xhr, status, error) {
                        $("#err").html("Dogodila se greska"+xhr.status).removeClass('nev');
                    }
                });
                // this.dohvati()
                // this.dohvati()
                // this.dohvati()
            },

            openPanel: function(id) {
                this.resetHolders();
                this.orderPodaci.clicked = true;
                for (var i = 0; i < this.orderBazaPodaci.length; i++) {
                    if (this.orderBazaPodaci[i]['id'] == id) {
                        this.orderPodaci.id = this.orderBazaPodaci[i]['id'];
                        this.orderPodaci.user.firstName = this.orderBazaPodaci[i].user.firstName;
                        this.orderPodaci.user.lastName = this.orderBazaPodaci[i].user.lastName;
                        this.orderPodaci.price = this.orderBazaPodaci[i]['price'];
                        this.orderPodaci.status_id = this.orderBazaPodaci[i]['status']['id'];
                        this.orderPodaci.picture = this.orderBazaPodaci[i]['picture']['file'];
                        this.orderPodaci.created_at = this.orderBazaPodaci[i]['created_at'];
                        this.orderPodaci.user.address = this.orderBazaPodaci[i].user.address;
                        this.orderPodaci.user.tel = this.orderBazaPodaci[i].user.tel;
                        this.orderPodaci.user.city = this.orderBazaPodaci[i].user.city;
                        this.orderPodaci.user.postNumber = this.orderBazaPodaci[i].user.postNumber;
                    }
                }
            },
            resetHolders: function() {
                $('#err').html("").addClass('nev');
                $('#success').html("").addClass('nev');
            },
            potvrdi: function(id) {
                for (i = 0; i < this.orderBazaPodaci.length; i++) {
                    if (this.orderBazaPodaci[i]['id'] == id) {
                        this.orderBazaPodaci[i]['status']['id'] = 2;
                    }
                }
            },
            dohvati : function(){
              var self = this;
              $.ajax({
                url: window.base_url+'/custom-case',
                type:'GET',
                dataType: 'json',
                success: function(data){
                  for(var i=0;i<data.cases.length;i++){
                    data.cases[i].picture.file = 'http://localhost/blacksheep/public/assets/pages/img/products/'+data.cases[i].picture.file;
                  }
                  self.orderBazaPodaci = data.cases;

                },
                error: function(xhr, status, error){

                }
              });
            }

        },
        beforeMount(){
          this.dohvati()

        }
    }
    var orderData = {
        filter :'1',
        clicked : false,
        id : 1,
        created_at : new Date().toLocaleDateString(),
        comment : 'Nema',
        picture_id : 1,
        user_id : 1,
        amount : 1,
        user : {
          id : 1,
          firstName : 'pera',
          lastName : 'mika',
          email : 'hehe',
          phone : '060',
          address: 'Vojvode',
          city : 'city',
          postNumber: '123123',
        },
        picture : {
          id : 1,
          created_at : new Date().toLocaleDateString(),
          file : 'pera.jpg',
          alt : 'pera'
        },
        status : {
          id : 1,
          created_at : new Date().toLocaleDateString(),
          name : 'Neobradjena'
        }

    }
    /* imp */
    var orderDbData = {
      cases: [{
        filter :'1',
        clicked : false,
        id : 1,
        created_at : new Date().toLocaleDateString(),
        comment : 'Nema',
        picture_id : 1,
        user_id : 1,
        amount : 1,
        user : {
          id : 1,
          firstName : 'pera',
          lastName : 'mika',
          email : 'hehe',
          phone : '060',
          address: 'Vojvode',
          city : 'city',
          postNumber: '123123',
        },
        picture : {
          id : 1,
          created_at : new Date().toLocaleDateString(),
          file : 'pera.jpg',
          alt : 'pera'
        },
        status : {
          id : 1,
          created_at : new Date().toLocaleDateString(),
          name : 'Neobradjena'
        }
      }]

    }
</script>
<style lang="css">
</style>
