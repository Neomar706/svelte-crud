<script>
  import { v4 } from "uuid";
  import swal from "sweetalert";

  let products = [];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: "",
  };

  let editStatus = false;

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageURL: "",
    };
  };

  const addProduct = () => {
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    };

    products = products.concat(newProduct);
    cleanProduct();
  };

  const updateProduct = () => {
    const updatedProduct = {
      id: product.id,
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    };

    const productIndex = products.findIndex((p) => p.id === product.id);
    products[productIndex] = updatedProduct;
    cleanProduct();
    editStatus = false;
  };

  const onSubmitHandle = (e) => {
    if (!editStatus) {
      addProduct();
    } else {
      updateProduct();
    }
  };

  const deleteProduct = (id) => {
    products = products.filter((product) => product.id !== id);
  };

  const editProduct = (productEdited) => {
    product = productEdited;
    editStatus = true;
  };

  const isAlertDetele = (i) => {
    swal({
      title: "Delete",
      text: "Are you sure you want to eliminate?",
      icon: "warning",
      buttons: ["NO", "YES"],
    }).then((res) => {
      if (res) {
        swal({
          text: "successfully deleted",
          icon: "success",
        });
        deleteProduct(i);
      }
    });
  };
</script>

<main>
  <div class="container">
    <div class="row pt-5">
      <div class="col-md-6">
        <div class="card mt-2">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandle}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  id="product-name"
                  placeholder="Product Name"
                  class="form-control" />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id="product-description"
                  rows="4"
                  placeholder="Products description"
                  class="form-control" />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  type="url"
                  id="product-image-url"
                  placeholder="https://ejemplo.com"
                  class="form-control" />
              </div>
              <div class="form-group">
                <select
                  bind:value={product.category}
                  id="category"
                  class="form-control">
                  <option value="laptops">Laptops</option>
                  <option value="peripherials">Peripherials</option>
                  <option value="servers">Servers</option>
                </select>
              </div>
              <div class="form-group">
                <button class="btn btn-success rounded">
                  {#if !editStatus}Save product{:else}Update product{/if}
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
      
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    src="images/no-products-found.jpg"
                    alt=""
                    class="img-fluid ml-2" />
                {:else}
                  <img src={product.imageURL} alt="" class="img-fluid ml-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                  <p class="card-text">{product.description}</p>
                  <button
                    on:click={isAlertDetele(product.id)}
                    class="btn btn-danger rounded">
                    Detele
                  </button>
                  <button
                    on:click={editProduct(product)}
                    class="btn btn-secondary rouded">
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</main>
