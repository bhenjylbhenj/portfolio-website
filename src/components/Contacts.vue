<template>
  <div id="contact" class="container my-5">
    <div class="row">
      <div
        class="col-12 col-md-6 d-none d-md-block d-flex justify-content-center"
      >
        <img src="/images/img3.jpg" class="img-fluid" />
      </div>
      <div
        class="col-12 col-md-6 d-flex justify-content-around"
        id="contact-form"
      >
        <div id="form">
          <h4>Contact Us</h4>
          <form class="text-center">
            <input
              class="d-block"
              type="text"
              placeholder="fullname"
              id="fullname"
            />
            <input
              class="d-block"
              type="email"
              placeholder="email"
              id="email"
            />
            <textarea placeholder="message" id="message" rows="3"></textarea>
            <button
              class="d-block btn"
              type="button"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            >
              Contact Me
            </button>
            <div
              class="modal fade"
              id="exampleModal"
              tabindex="-1"
              role="dialog"
              aria-labelledby="exampleModalLabel"
              aria-hidden="true"
            >
              <div class="modal-dialog" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">
                      Message Box
                    </h5>
                  </div>
                  <div class="modal-body">Thank you for contacting us!</div>
                  <div class="modal-footer">
                    <button
                      type="button"
                      class="btn btn-secondary"
                      data-bs-dismiss="modal"
                    >
                      Close
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
        <div id="contact-div">
          <div class="contact-section">
            <p class="head">Contact</p>
            <p>super@bhenj.com</p>
          </div>
          <div class="contact-section">
            <p class="head">Based in</p>
            <p>San Francisco,</p>
            <p>California</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
