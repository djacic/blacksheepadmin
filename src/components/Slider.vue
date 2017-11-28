<template>
  <div>
        <div class='col-md-12 title'>Admin - Slajder</div>
    <div class='col-md-12'>
      <table class='table table-hover table-bordered'>
        <thead>
          <th>Fajl</th>
          <th>Trenutna</th>
        </thead>
        <tbody>
        <tr>
          <td><input type="file" id="file" ref="myFiles"  @change="changeIt(1)"></td>
          <td><img src='http://www.blacksheepmobstore.com/final/public/assets/pages/img/shop-slider/slide/1.jpeg' width='100'/></td>
        </tr>
        <tr>
          <td><input type="file" id="file2" ref="myFiles2"  @change="changeIt(2)"></td>
          <td><img src='http://www.blacksheepmobstore.com/final/public/assets/pages/img/shop-slider/slide/2.jpeg' width='100'/></td>
        </tr>
        <tr>
          <td><input type="file" id="file3" ref="myFiles3"  @change="changeIt(3)"></td>
          <td><img src='http://www.blacksheepmobstore.com/final/public/assets/pages/img/shop-slider/slide/3.jpeg' width='100'/></td>
        </tr>
        <tr>
          <td><input type="file" id="file4" ref="myFiles4"  @change="changeIt(4)"></td>
          <td><img src='http://www.blacksheepmobstore.com/final/public/assets/pages/img/shop-slider/slide/4.jpeg' width='100'/></td>
        </tr>
      </tbody>
      </table>
      <div id='err' class='alert alert-danger nev'></div>
      <div class='alert alert-info nev' id='feedback'></div>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      files:[],
      slike: sliderImages
    }
  },
  methods: {
    changeIt(x) {
        var id ='';

      switch (x) {
        case 1:
        var fileSelect = document.getElementById('file');
          break;
        case 2:
        var fileSelect = document.getElementById('file2');
          break;
        case 3:
        var fileSelect = document.getElementById('file3');
          break;
        case 4:
        var fileSelect = document.getElementById('file4');
          break;
        default: break;
      }
        var files = fileSelect.files;
        var formData = new FormData();
        var file = files[0];
        formData.append('picture', file);
		formData.append('_method', 'PATCH');
        var self = this;
		for (var pair of formData.entries()) {
    console.log(pair[0]+ ', ' + pair[1]);
}
        $.ajax({
        url: window.base_url+'/update-slider/'+x,
        type: 'POST',
        processData: false,
		contentType: false,
        cache: false,
        data: formData,
        dataType: 'json',
        success: function(data){
          // self.ucitaj();
        },
        error: function(xhr, status, error){
          switch (xhr.status) {
            case 200:
              $('#feedback').html('Uspesna izmena').removeClass('nev');
              break;
            default:
            $('#err').html('Dogodila se greska - '+xhr.status+", poslat [id]: "+x).removeClass('nev');
              break;
          }
        }
      });


  },
  beforeMount(){
    this.ucitaj()
  }
  }
}
var sliderImages = {
  slides: [{
    id : 1,
    img: 'https://static.pexels.com/photos/17679/pexels-photo.jpg'
  },
  {
    id : 2,
    img: 'https://static.pexels.com/photos/17679/pexels-photo.jpg'
  },
  {
    id:3,
    img: 'https://static.pexels.com/photos/17679/pexels-photo.jpg'
  },
  {
    id:4,
    img: 'https://static.pexels.com/photos/17679/pexels-photo.jpg'
  }]
}
</script>

<style lang="css">
</style>
