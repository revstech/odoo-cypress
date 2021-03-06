
Odoo Framework E2E Testing using Cypress
=========================================


Using **odoo-cypress**  run your Odoo E2E tests on any CI is very simple. 

Fast, easy and reliable testing for anything in odoo.


Example
-------

Test Case : Workflow Quotation:

- Select partner
- Select first product : CONS_DEL01] Server
- Check value of unit_price after onchange product 
- Select second product : [CPUi5] Processor Core i5 2.70 Ghz
- Check value of unit_price after onchange product 
- Save quoation
- Check the amount_total of Quotation 
- Confirm quoation
- check state should have  'Sales Order' value

You can do this test case with this code : 
 
    it('Quotation',  function(){
       cy.MainMenu('Sales','sale.sale_menu_root')
       cy.SubMenu('Orders','sale.menu_sale_order')
       cy.Button('Create')
       cy.M2O_SetValue('partner_id','Agrolait')
       cy.M2O_SetValue_O2M('order_line','product_id','[CONS_DEL01] Server')
       cy.O2M_CheckValue('order_line',1,'price_unit','35,319.51')
       cy.M2O_SetValue_O2M('order_line','product_id','[CPUi5] Processor Core i5 2.70 Ghz')
       cy.O2M_CheckValue('order_line',2,'price_unit','1,236.18')
       cy.Button('Save')
       cy.CheckValue('amount_total','36,555.69')
       cy.Button('Confirm Sale')
       cy.Waiting(2000)
       cy.State_CheckValue('state','Sales Order')
    })



Installation
------------

To use this project locally as a dev dependency for your project: 

```bash
git clone https://github.com/borni-dhifi/odoo-cypress
cd odoo-cypress
npm install cypress
```


Run Test 
--------

Modify file cypress.env.json with your odoo server param:

```bash
    {
     "odoo_url": "http://localhost:8070",
     "database": "cypress2",
     "user": "admin",
     "password": "admin"
    }
```


```bash
##launch the Cypress Test Runner
##in the GUI mode
npm run cy_open
## runs all tests and exits with the total number of
## failures across all tests
npm run cy_run
### runs all tests in specific browser
npm run cy_run_chrome
### sends test results, videos, screenshots
### to Cypress dashboard
npm run cy_run_record
```

Write tests 
-----------

To write your tests just edit the file  odoo-cypress/cypress/integration/odoo_spec.js and add your tests case : 

    it('Your Test Case 1',  function(){ 
       //...
    }) 
    
    it('Your Test Case 2',  function(){ 
       //...
    }) 
    
or you can create new file odoo-cypress/cypress/integration/your_file_name.js and write your tests.

    describe('Test Invoice Workflow', function(){
      context('Invoice', function(){ 
        it('Create and validate invoice', function(){ 
         //..
       })
      })
    })

Continuous Integration
-----------------------

Running your Odoo E2E tests on any CI is very simple : Jenkins, TravisCI, CircleCI, GitLab .... is the same as running it locally.

- Travis

Example .travis.yml config file

    script:
      - cypress run --record

- CircleCI

Example circle.yml config file

    test:
      override:
        - cypress run --record

- Gitlab 

Example .gitlab-ci.yml file

    image: cypress/base
    cypress-e2e:
      script:
        - npm install
        - $(npm bin)/cypress run
      
      
- Docker

For Docker You can either start with a base image or with an image that already includes Cypress tool.

Use the official image <a href="https://hub.docker.com/r/cypress/base/">cypress/base</a>. A typical Dockerfile would look like this:

    FROM cypress/base
    RUN npm install
    RUN $(npm bin)/cypress run
  

Recording Tests in CI
----------------------


Tou can record your tests running and make them available in <a href="https://on.cypress.io/dashboard">Cypress Dashboard</a>


Todo
----

- Improve the waiting method.
- Add more methods for framework .
- I think is better to remove XML_ID from MainMenu/SubMenu

Ressources
------------

- <a href="https://www.cypress.io">Cypress.io</a>
- <a href="https://docs.cypress.io/guides/guides/continuous-integration.html">Continuous Integration</a>
- <a href="https://docs.cypress.io/guides/guides/continuous-integration.html#Recording-Tests-in-CI">Recording Tests in CI</a>




