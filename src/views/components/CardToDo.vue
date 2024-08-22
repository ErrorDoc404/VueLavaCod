<template>
  <div class="card bg-gradient-dark move-on-hover">
    <div class="card-body">
      <div class="d-flex">
        <h5 class="mb-0 text-white">In Queue</h5>
        <div class="ms-auto">
          <h1 class="mb-0 text-white text-end mt-n2">7</h1>
          <p class="mb-0 text-sm text-white">songs</p>
        </div>
      </div>
      <p class="mb-0 text-white">Current Song</p>
      <p class="mb-0 text-white">Requester</p>
    </div>
    <a
      href="javascript:;"
      class="py-1 text-center w-100"
      data-bs-toggle="tooltip"
      data-bs-placement="top"
      title="Show More"
    >
      <i class="text-white fas fa-chevron-down"></i>
    </a>
  </div>
</template>
