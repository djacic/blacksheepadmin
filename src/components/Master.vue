<template>
  <!DOCTYPE html>
<html lang="en">
   <head>
      <meta charset="utf-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
      <meta name="description" content="Panel za upravljanje sadrzajem sajta blacksheepmobstore.com">
      <meta name="author" content="Milos Djacic">
      <title>Admin Panel <!-- ovde title --></title>
   </head>
   <body class="fixed-nav sticky-footer bg-dark" id="page-top">
      <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top" id="mainNav">
         <a class="navbar-brand" href="index.html">BS Panel za upravljanje</a>
         <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
         <span class="navbar-toggler-icon"></span>
         </button>
         <div class="collapse navbar-collapse" id="navbarResponsive">
            <ul class="navbar-nav navbar-sidenav" id="exampleAccordion">
              <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Statistika">
                 <a class="nav-link">
                 <i class="fa fa-dashboard"></i>
                 <router-link style='color:#868E96; text-decoration:none' to="/stats">Statistika</router-link>
                 </a>
              </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Kategorije">
                  <a class="nav-link">
                  <i class="fa fa-balance-scale"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/categories">Kategorije</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Brendovi">
                  <a class="nav-link">
                  <i class="fa fa-snowflake-o"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/brands">Brendovi</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Tipovi">
                  <a class="nav-link">
                  <i class="fa fa-hashtag"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/types">Tipovi</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Proizvodi">
                  <a class="nav-link">
                  <i class="fa fa-magic"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/products">Proizvodi</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Boje">
                  <a class="nav-link">
                  <i class="fa fa-check"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/colors">Boje</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Porudžbine">
                  <a class="nav-link">
                  <i class="fa fa-reorder"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/orders">Porudžbine</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Custom">
                  <a class="nav-link">
                  <i class="fa fa-suitcase"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/custom-case">Custom Case</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Slajder">
                  <a class="nav-link">
                  <i class="fa fa-image"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/slider">Slajder</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Dostava">
                  <a class="nav-link">
                  <i class="fa fa-car"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/delivery">Dostava</router-link>
                  </a>
               </li>
               <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Akcija">
                  <a class="nav-link">
                  <i class="fa fa-smile-o"></i>
                  <router-link style='color:#868E96; text-decoration:none' to="/special">Akcija</router-link>
                  </a>
               </li>
               <!-- <li class="nav-item" data-toggle="tooltip" data-placement="right" title="Link">
                  <a class="nav-link" href="#">
                  <i class="fa fa-fw fa-link"></i>
                  <span class="nav-link-text">Link</span>
                  </a>
               </li> -->
            </ul>
            <ul class="navbar-nav sidenav-toggler">
               <li class="nav-item">
                  <a class="nav-link text-center" id="sidenavToggler">
                  <i class="fa fa-fw fa-angle-left"></i>
                  </a>
               </li>
            </ul>
         </div>
      </nav>
      <div class="content-wrapper">
         <div class="container-fluid">
           <div class='col-md-12 pull-left'>
             <!-- ovde title stranice -->
           </div>
           <router-view></router-view>
         </div>
         <!-- /.container-fluid-->
         <!-- /.content-wrapper-->
         <!-- <footer class="sticky-footer">
            <div class="container">
               <div class="text-center">
                  <small>Copyright © Black Sheep - Panel za upravljanje 2017</small>
               </div>
            </div>
         </footer> -->
         <!-- Scroll to Top Button-->
         <a class="scroll-to-top rounded" href="#page-top">
         <i class="fa fa-angle-up"></i>
         </a>
         <!-- Logout Modal-->
         <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog" role="document">
               <div class="modal-content">
                  <div class="modal-header">
                     <h5 class="modal-title" id="exampleModalLabel">Ready to Leave?</h5>
                     <button class="close" type="button" data-dismiss="modal" aria-label="Close">
                     <span aria-hidden="true">×</span>
                     </button>
                  </div>
                  <div class="modal-body">Select "Logout" below if you are ready to end your current session.</div>
                  <div class="modal-footer">
                     <button class="btn btn-secondary" type="button" data-dismiss="modal">Cancel</button>
                     <a class="btn btn-primary" href="login.html">Logout</a>
                  </div>
               </div>
            </div>
         </div>
         <!-- Bootstrap core JavaScript-->

         <!-- Custom scripts for this page-->
         <!-- <?php
         echo "<script> var base_url = '". url("/") .  "';</script>";
         ?> -->

          <!-- <script src="js/blacksheepadmin/src/ajax.js") }}"></script> -->

                   <!-- Custom scripts for this page-->

      </div>
   </body>
</html>
</template>
