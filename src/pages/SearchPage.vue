<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
    <form class="form-inline">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Find a Recipe" aria-label="Find a Recipe" aria-describedby="button-addon2">
        <div class="input-group-append">
          <b-button size="" class="mb-3">
            <b-icon icon="search" aria-hidden="true"></b-icon>
          </b-button>
        </div>
      </div>
      <div class="dropdown">
        <a class="btn btn-secondary dropdown-toggle" href="#" role="button" id="dropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown link
        </a>
        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </div>
      <div class="form-group">
        <div class="form-check-gluten-free">
          <input class="form-check-input" type="checkbox" id="gridCheck">
          <label class="form-check-label" for="gridCheck">
            Gluten Free
          </label>
        </div>
      </div>
      <div class="form-group">
        <div class="form-check-vegan">
          <input class="form-check-input" type="checkbox" id="gridCheck">
          <label class="form-check-label" for="gridCheck">
            Vegan
          </label>
        </div>
      </div>
      <div class="form-group">
        <div class="form-check-vegeterian">
          <input class="form-check-input" type="checkbox" id="gridCheck">
          <label class="form-check-label" for="gridCheck">
            Vegeterian
          </label>
        </div>
      </div>
    </form>
    
  </div>
</template>

<script>
  
</script>

