<template>
  <div>
        <div class='col-md-12 title' id='top'>Admin - Proizvodi</div>
  <div class='col-md-6 pull-left'>
    <!-- <div class='form-group col-md-6'><select v-model='forma.filterCategory' class='form-control'><option value=''>Izaberite kategoriju...</option><option v-for='cat in forma.categories' :value='cat.id'>{{ cat.name }}</option></select></div> -->
    <div class='form-group col-md-5'><input type='text' class='form-control' v-model='forma.searchQuery'/></div><div class='col-md-1'><button @click='searchForItem(forma.searchQuery)' class='btn btn-primary btn-xs'><i class='fa fa-search fa-2x'></i></button></div>
    <table class='table table-hover table-condensed'>
      <thead>
        <tr v-if='forma.filterCategory'>
        <th>Naziv</th>
        <!-- <th>Slika</th> -->
        <!-- <th>Izmeni</th> -->
        <th>Obrisi</th>
      </tr>
      </thead>
      <tbody>
        <tr v-for='p in podaci.products' v-if='p.is_active==1'> <!-- v-if='p.category_id == forma.filterCategory' 2badded-->
          <td>{{p.name}}</td>
          <!-- <td v-for='slika in p'><img :src='slika.file' width='50' :alt='slika.alt'/></td> -->
         <td><button type='button' class='btn btn-warning' @click='preEdit(p.id)'><i class='fa fa-edit'></i></button></td>
          <td><button type='button' class='btn btn-danger' @click='preRemove(p.id)'><i class='fa fa-remove'></i></button></td>
        </tr>
      </tbody>
    </table>
  </div>
  <div class='col-md-6 pull-left'>
      <hr>
    <h5>Unos proizvoda</h5>
    <table class='table table-striped'>
      <tr>
        <td><label for='name'>Naziv:</label></td>
        <td><input type='text' name='name' id='name' class='form-control' placeholder="Naziv proizvoda" v-model='forma.name' /></td>
        <td><i>Izdvojen?</i><input type='checkbox' class='form-control' v-model='forma.special'/></td>
      </tr>
      <tr>
        <td><label for='description'>Opis:</label></td>
        <td><textarea name='description' id='description' class='form-control' placeholder="Opis proizvoda" v-model='forma.description'/></td>
      </tr>
      <tr v-if='forma.is_offer == 0'>
        <td><label for='price'>Cena:</label></td>
        <td><input type='text' name='price' id='price' class='form-control' placeholder="Cena proizvoda" v-model='forma.price'/></td>
        <td><i>Akcija?</i><input type='checkbox' class='form-control' v-model='forma.is_offer'/></td>
      </tr>
      <tr v-else>
        <td><label for='price'>Cena:</label></td>
        <td><input type='text' name='price' id='price' class='form-control' placeholder="Akcijska cena proizvoda" v-model='forma.price'/></td>
        <td><span>Akcija?</span><input type='checkbox' class='form-control' v-model='forma.is_offer'/></td>
      </tr>
      <tr>
        <td><label for='picture'>Slika:</label></td>
        <td><input class='form-control' type='file' name='picture' id='file'/></td>
      </tr>
      <tr>
        <td><label for='color'>Boja?</label></td>
        <td><input type='checkbox' class='form-control chbWidth' v-model='forma.bojanJe'/></td>
      </tr>
      <tr v-if='forma.bojanJe'>
        <td>Izaberi:</td>
        <td>

          <div v-for='c in forma.dbcolors'><label class='label-for-check' v-bind:style={background:c.hex}><input type='checkbox' name='chooseColor' v-model='c.checked' :value='c.id' :id='c.id' class='form-control chbWidth  check-with-label'/></label></div>

        </td>
      </tr>
      <tr>
        <td><label for='brand'>Brend:</label></td>
        <td><select v-model='forma.brand_id' class='form-control' id='brand'><option value='0'>Izaberite brend</option><option :value='b.id' v-for='b in forma.brand' >{{b.name}}</option></select></td>
      </tr>
      <tr>
        <td><label for='tip'>Tip:</label></td>
        <td><select v-model='forma.type_id' class='form-control' id='tip'><option value='0'>Izaberite tip</option><option :value='t.id' v-for='t in forma.type' >{{t.name}}</option></select></td>
      </tr>
      <tr>
        <td align='center' colspan='2' v-if='forma.isInsert'><button type='button' @click='unesi' class='btn btn-success'>Unesi</button></td>
        <td v-else><button type='button' @click='edit' class='btn btn-success'>Izmeni</button>&nbsp;<button @click='switchToInsert' class='btn btn-info'><i class='fa fa-plus'></i></button></td>
      </tr>
      <tr>
        <td colspan='2'><div class='centered' v-for='err in errors' v-if='hasErrors'>{{err}}<br/></div>
        <div class='centered' id='feedback'></div>
        </td>
      </tr>
    </table>


<!-- <span v-for='col in forma.dbcolors' v-if='col.checked'>{{col.hex}}</span> -->
  </div>
</div>
</template>
<script>
    export default {
        data() {
            return {
                podaci: sviPodaci,
                unos: dodavanjePodataka,
                brisanje: brisanjePodataka,
                izmena: izmenaPodataka,
                forma: formData,
                errors: [],
                hasErrors: false
            }
        },
        methods: {
          searchForItem: function(query) {
    sviPodaci.products = window.kopija.slice(0);
    for (var i = 0; i < sviPodaci.products.length; i++) {
        if (sviPodaci.products[i]['name'].toUpperCase().indexOf(query.toUpperCase()) == -1) {
          sviPodaci.products.splice(i, 1);
          --i;
        }
    }

},
switchToInsert: function(){
  this.forma.name = '';
  this.forma.description = '';
  this.forma.price = [];
  this.forma.bojanJe = false;
  this.forma.brand_id = 0;
  this.forma.type_id = 0;
  this.forma.isInsert=true;
  this.errors = []
},
          dohvatiKategorije : function(){
            $.ajax({
              url: window.base_url+'/categories',
              type: 'GET',
              dataType: "json",
              success: function(data) {

                  formData.categories = data.categories;
              },
              error: function(xhr, status, error) {
                  $("#feedback").html("Dogodila se greska").removeClass('nev');
              }
            });
          },
          dohvatiBrendTip : function(){
            $.ajax({
              url: window.base_url+'/categories',
              type: 'GET',
              dataType: "json",
              success: function(data) {
                  formData.brand = data.brands;
                  formData.type = data.types;
              },
              error: function(xhr, status, error) {
                  $("#feedback").html("Dogodila se greska").removeClass('nev');
              }
            });
          },
          dohvatiBoje : function(){
            $.ajax({
              url: window.base_url+'/colors',
              type: 'GET',
              dataType: "json",
              success: function(data) {
                  formData.dbcolors = data.colors;
              },
              error: function(xhr, status, error) {
                  $("#feedback").html("Dogodila se greska").removeClass('nev');
              }
            });
          },
          formReset : function(){
            this.forma.name = '';
            this.forma.description = '';
            this.forma.price = [];
            // document.getElementById('file').files = [];
            this.forma.bojanJe = false;
            this.forma.brand_id = 0;
            this.forma.type_id = 0;
          },
            unesi: function() {
                this.forma.bojanJe ? this.forma.checked = [] : false
                this.errors = [];
                this.forma.checked = [];
                var rePrice = /^[0-9]{1,10}$/;
                if (!rePrice.test(this.forma.price)) {
                    this.errors.push('Cena nije u dobrom formatu. [123]');
                }
                if (this.forma.brand_id == 0) {
                    this.errors.push('Brend je obavezan.');
                }
                /* BOJE */
                for (var i = 0; i < this.forma.dbcolors.length; i++) {
                    if (this.forma.dbcolors[i].checked) {
                        this.forma.checked.push(this.forma.dbcolors[i].id);
                    }
                }
                if (this.forma.bojanJe) {
                    if (this.forma.checked.length == 0) {
                        this.errors.push('Izaberite bar jednu boju!');
                    }
                }
                if (document.getElementById('file').files.length != 0) {
                  var fileSelect = document.getElementById('file').files[0];
                  if (!fileSelect.type.match('image.*')) {
                    this.errors.push('Fajl mora biti slika');
                }

                }
                /* BOJE */
                this.hasErrors = this.errors.length > 0;
                if (!this.hasErrors) {
                    this.unos.name = this.forma.name;
                    this.unos.description = this.forma.description;
                    this.unos.is_active = 1; //Postaje aktivan
                    if (this.forma.is_offer) this.unos.is_offer = 1;
                    this.unos.price = this.forma.price;
                    this.unos.brand_id = this.forma.brand_id;
                    if (this.forma.special) this.unos.special = 1;
                    if (this.forma.type_id != null) this.unos.type_id = this.forma.type_id;
                    this.unos.checked = this.forma.checked;
                    if (document.getElementById('file').files.length == 0) {
                      var formData = new FormData();
                      formData.append('picture', null);
                      formData.append('brand_id', this.unos.brand_id);
                      formData.append('type_id', this.unos.type_id);
                      formData.append('colors', JSON.stringify(this.unos.checked));
                      formData.append('description', this.unos.description);
                      formData.append('is_active', this.unos.is_active);
                      formData.append('is_offer', this.unos.is_offer);
                      formData.append('name', this.unos.name);
                      formData.append('price', this.unos.price);
                      formData.append('special', this.unos.special);
                    } else {
                      var fileSelect = document.getElementById('file');
                      var files = fileSelect.files;
                      var formData = new FormData();
                      var file = files[0];
                        formData.append('picture', file);
                        formData.append('brand_id', this.unos.brand_id);
                        formData.append('type_id', this.unos.type_id);
                        formData.append('colors', JSON.stringify(this.unos.checked));
                        formData.append('description', this.unos.description);
                        formData.append('is_active', this.unos.is_active);
                        formData.append('is_offer', this.unos.is_offer);
                        formData.append('name', this.unos.name);
                        formData.append('price', this.unos.price);
                        formData.append('special', this.unos.special);
                    }


                    var insertData = {
                      brand_id : this.unos.brand_id,
                      type_id : this.unos.type_id,
                      colors : this.unos.checked,
                      description: this.unos.description,
                      is_active: this.unos.is_active,
                      is_offer: this.unos.is_offer,
                      name: this.unos.name,
                      picture: this.unos.picture,
                      price: this.unos.price,
                      special: this.unos.special
                    }
                    // ajax
                    $.ajax({
                        url: window.base_url+'/products',
                        type: 'POST',
                        dataType: "json",
                        data: formData,
                        contentType: false,
                        cache: false,
                        processData:false,
                        success: function(data) {
                            $("#feedback").html("Proizvod je uspešno unet!");
                        },
                        error: function(xhr, status, error) {
                            switch (xhr.status) {
                                case 201:
                                    $("#feedback").html("Proizvod je uspešno unet!");
                                    break;
                                default:
                                    $("#feedback").html("Dogodila se greska - " + xhr.status);
                                    break;
                            }
                        }
                    });
                    this.dohvati()
                    this.dohvati()
                    this.dohvati()
                } else {
                    return false;
                }
            },
            preEdit: function(x) {
              location.hash = "#top";
              this.forma.id = x;
                this.forma.isInsert = false;
                this.forma.bojanJe = false;
                for (var n = 0; n < this.forma.dbcolors.length; n++) {
                    this.forma.dbcolors[n].checked = false;
                  }
                for (var i = 0; i < sviPodaci.products.length; i++) {
                    if (sviPodaci.products[i]['id'] == x) {
                        this.forma.id = sviPodaci.products[i]['id'];
                        this.forma.name = sviPodaci.products[i]['name'];
                        this.forma.description = sviPodaci.products[i]['description'];
                        sviPodaci.products[i]['is_offer'] == 1 ? this.forma.is_offer = 1 : this.forma.is_offer = 0;
                        this.forma.price = sviPodaci.products[i]['prices'][0].price;
                        if (sviPodaci.products[i]['colors'].length > 0) {

                            this.forma.bojanJe = true;

                            for (var j = 0; j < this.forma.dbcolors.length; j++) {
                                for(var k = 0; k<sviPodaci.products[i]['colors'].length;k++){
                                  if (sviPodaci.products[i]['colors'][k]['id'] == this.forma.dbcolors[j]['id'] ) {
                                      this.forma.dbcolors[j]['checked'] = true;
                                  }
                                }


                            }

                        }else{
                          this.forma.bojanJe = false;
                        }
                        this.forma.brand_id = sviPodaci.products[i]['brand_id'];
                        this.forma.type_id = sviPodaci.products[i]['type_id'];
                    }
                }
            },
            edit: function() {
              this.unos.id = this.forma.id;
              this.errors = [];
              this.forma.checked = [];
              var rePrice = /^[0-9]{1,7}$/;
              if (!rePrice.test(this.forma.price)) {
                  this.errors.push('Cena nije u dobrom formatu. [123]');
              }
              if (this.forma.brand_id == 0) {
                  this.errors.push('Brend je obavezan.');
              }
              for (var i = 0; i < this.forma.dbcolors.length; i++) {
                  if (this.forma.dbcolors[i].checked) {
                      this.forma.checked.push(this.forma.dbcolors[i].id);
                  }
              }
			  if (document.getElementById('file').files.length != 0) {
                  var fileSelect = document.getElementById('file').files[0];
                  if (!fileSelect.type.match('image.*')) {
                    this.errors.push('Fajl mora biti slika');
                }
              }
              this.hasErrors = this.errors.length > 0;
              if (!this.hasErrors) {
                  this.unos.name = this.forma.name;
                  this.unos.description = this.forma.description;
                  this.unos.is_active = 1; //Postaje aktivan
                  if (this.forma.is_offer) this.unos.is_offer = 1;
                  this.unos.price = this.forma.price;
                  this.unos.brand_id = this.forma.brand_id;
                  if (this.forma.special) this.unos.special = 1;
                  if (this.forma.type_id != null) this.unos.type_id = this.forma.type_id;
                  this.unos.checked = this.forma.checked;
                  if (document.getElementById('file').files.length == 0) {
                    var formData = new FormData();
                    formData.append('picture', null);
                    formData.append('brand_id', this.unos.brand_id);
                    formData.append('type_id', this.unos.type_id);
                    formData.append('colors', JSON.stringify(this.unos.checked));
                    formData.append('description', this.unos.description);
                    formData.append('is_active', this.unos.is_active);
                    formData.append('is_offer', this.unos.is_offer);
                    formData.append('name', this.unos.name);
                    formData.append('price', this.unos.price);
                    formData.append('special', this.unos.special);
                    formData.append('_method', 'PATCH');
                  } else {
                    var fileSelect = document.getElementById('file');
                    var files = fileSelect.files;
                    var formData = new FormData();
                    var file = files[0];
                      formData.append('picture', file);
                      formData.append('brand_id', this.unos.brand_id);
                      formData.append('type_id', this.unos.type_id);
                      formData.append('colors', JSON.stringify(this.unos.checked));
                      formData.append('description', this.unos.description);
                      formData.append('is_active', this.unos.is_active);
                      formData.append('is_offer', this.unos.is_offer);
                      formData.append('name', this.unos.name);
                      formData.append('price', this.unos.price);
                      formData.append('special', this.unos.special);
                      formData.append('_method', 'PATCH');


                  }
                  for (var pair of formData.entries()) {
    console.log(pair[0]+ ', ' + pair[1]);
}
// console.log(this.unos.id);
                  // var editData = {
                  //   brand_id : this.unos.brand_id,
                  //   type_id : this.unos.type_id,
                  //   colors : this.unos.checked,
                  //   description: this.unos.description,
                  //   is_active: this.unos.is_active,
                  //   is_offer: this.unos.is_offer,
                  //   name: this.unos.name,
                  //   picture: this.unos.picture,
                  //   price: this.unos.price,
                  //   special: this.unos.special
                  // }
                  // var data = {
                  //   brand_id : this.unos.brand_id,
                  //   type_id : this.unos.type_id,
                  //   colors : this.unos.checked,
                  //   description: this.unos.description,
                  //   is_active: this.unos.is_active,
                  //   is_offer: this.unos.is_offer,
                  //   name: this.unos.name,
                  //   price: this.unos.price,
                  //   special: this.unos.special
                  // }
                  // console.log(data);

                  $.ajax({
                      url: window.base_url+'/products/'+this.unos.id,
                      type: 'POST',
                      dataType: "json",
                      contentType: false,
                      cache: false,
                      processData:false,
                      data: formData,
                      success: function(data) {
                          $("#feedback").html("Proizvod je uspešno izmenjen!");
                      },
                      error: function(xhr, status, error) {
                          switch (xhr.status) {
                              case 201:
                                  $("#feedback").html("Proizvod je uspešno izmenjen!");
                                  break;
                              default:
                                  $("#feedback").html("Dogodila se greska - " + xhr.status);
                                  break;
                          }
                      }
                  });
                  this.dohvati()
                  this.dohvati()
                  this.dohvati()

            }},
            dohvati: function() {
    $.ajax({
        url: window.base_url+'/products',
        type: 'GET',
        dataType: "json",
        success: function(data) {
            sviPodaci.products = data.products;
            window.kopija = sviPodaci.products.slice(0);

        },
        error: function(xhr, status, error) {
            $("#feedback").html("Greska u dohvatanju podataka iz baze!");
        }
    });
},
preRemove: function(x) {
    $.ajax({
        url: window.base_url+'/products/'+x,
        type: 'DELETE',
        dataType: "json",
        success: function(data) {

        },
        error: function(xhr, status, error) {
            $("#feedback").html("Dogodila se greska - "+ xhr.status + "<br/> Poslat je [id]: "+x).removeClass('nev');
        }
    });
    this.dohvati()
    this.dohvati()
    this.dohvati()
    this.formReset()
},
        },
        beforeMount() {
            this.dohvati()
            this.dohvatiBoje()
            this.dohvatiBrendTip()
            this.dohvatiKategorije()
        }
    }
    var sviPodaci = {
        filterCategory: "",
        products: [{
            id: 1,
            name: 'Product',
            description: 'Opis',
            is_active: 1,
            is_offer: 1,
            special: 0,
            brand_id: 1,
            picture_id: 1,
            type_id: 0,
            brand: [{
                id: null,
                name: null
            }],
            picture: [],
            type: [{
                id: null,
                name: null
            }],
            price: [], //ovisno od is_offer
            colors: [{
                id: 1,
                hex: '#ff0000',
                checked: true
            }, {
                id: 2,
                hex: '#ff0000',
                checked: true
            }]
        }]
    }
    var formData = {
        id : 1,
        name: '',
        description: '',
        is_active: 1,
        is_offer: 0,
        special: 0,
        brand_id: 0,
        category_id: 1,
        picture_id: 0,
        type_id: null,
        brand: [{
            id: 1,
            name: 'brend'
        }],
        picture: [],
        type: [{
            id: 1,
            name: 'tip'
        }],
        price: '',
        dbcolors: [{
            id: 1,
            hex: '#ff0000',
            checked: false
        }, {
            id: 2,
            hex: '#0099cc',
            checked: false
        }, {
            id: 3,
            hex: '#ee55cc',
            checked: false
        }, {
            id: 4,
            hex: '#ffffff',
            checked: false
        }, {
            id: 5,
            hex: '#000000',
            checked: false
        }, {
            id: 6,
            hex: '#cccccc',
            checked: false
        }, {
            id: 7,
            hex: '#999999',
            checked: false
        }],
        checked: [],
        bojanJe: false,
        isInsert: true,
        filterCategory: "",
        searchQuery: "",
        categories: [{
           id : 1,
           name : 'Maske'
        }]
    }
    var dodavanjePodataka = {
         id: '',
        name: '',
        description: '',
        is_active: 1,
        is_offer: 0,
        special: 0,
        brand_id: 0,
        picture_id: 0,
        type_id: 0,
        picture: [],
        price: 23,
        colors: [{
            id: 1
        }],
        checked: []
    }
    var brisanjePodataka = {
        id: ''
    }
    var izmenaPodataka = {
      id : 1,
      name: '',
      description: '',
      is_active: 1,
      is_offer: 0,
      special: 0,
      brand_id: 0,
      picture_id: 0,
      type_id: 0,
      picture: [],
      price: 123,
      colors: [{
          id: 1
      }],
      checked: []
    }
</script>
