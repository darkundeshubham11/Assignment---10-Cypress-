# LetsKodeIt Practice Page Tests

This project contains automated tests for the [LetsKodeIt Practice Page](https://www.letskodeit.com/practice) using Cypress. The tests cover various features available on the practice page to ensure their functionality.

## Prerequisites

- Node.js (https://nodejs.org/)
- npm (Node Package Manager, comes with Node.js)

## Setup

1. Clone the repository or download the project files.

2. Open a terminal and navigate to the project directory.

3. Install the required dependencies:
  
   npm install
Open Cypress Test Runner:

npx cypress open

Test Script
The Cypress test script is located in the cypress/integration directory. The test script is named practice_spec.js.

Tests Included
Radio Buttons:
Tests the functionality of radio buttons.
Checkboxes:
Tests the functionality of checkboxes.
Dropdown Select:
Tests selecting values from a dropdown menu.
Multiple Select:
Tests selecting multiple values from a multi-select dropdown.
Switch Window:
Tests the functionality of opening a new window.
Switch Tab:
Tests the functionality of opening a new tab.
Alert:
Tests handling JavaScript alerts.
Confirm Alert:
Tests handling JavaScript confirm alerts.
iFrame:
Tests interaction with elements inside an iframe.
Hide/Show:
Tests hide/show functionality of elements.
Mouse Hover:
Tests mouse hover functionality.
Dynamic Dropdown:
Tests the dynamic autocomplete dropdown.
**Sample Test Code**

///<reference types="cypress"/>
describe('LetsKodeIt Practice Page Tests', () => {
    beforeEach(() => {
        // Visit the practice page before each test
        cy.visit('https://www.letskodeit.com/practice')
    })

    it('should test radio buttons', () => {
        // Select a radio button
        cy.get('#bmwradio').check().should('be.checked')
        cy.get('#benzradio').check().should('be.checked')
        cy.get('#hondaradio').check().should('be.checked')
    })

    it('should test checkboxes', () => {
        // Select checkboxes
        cy.get('#bmwcheck').check().should('be.checked')
        cy.get('#benzcheck').check().should('be.checked')
        cy.get('#hondacheck').check().should('be.checked')
        cy.get('#bmwcheck').uncheck().should('not.be.checked')
    })

    it('should test drop-down select', () => {
        // Select a value from the dropdown
        cy.get('#carselect').select('bmw').should('have.value', 'bmw')
        cy.get('#carselect').select('benz').should('have.value', 'benz')
        cy.get('#carselect').select('honda').should('have.value', 'honda')
    })

    it('should test multiple select', () => {
        // Select multiple options from the multi-select box
        cy.get('#multiple-select-example').select(['apple', 'orange']).invoke('val').should('deep.equal', ['apple', 'orange'])
    })

    it('should test switch window', () => {
        // Handle new window/tab opening
        cy.get('#openwindow').invoke('removeAttr', 'target').click()
        cy.url().should('include', 'letskodeit.com')
        cy.go('back')
    })

    it('should test switch tab', () => {
        // Handle new tab opening
        cy.get('#opentab').invoke('removeAttr', 'target').click()
        cy.url().should('include', 'letskodeit.com')
        cy.go('back')
    })

    it('should test alert', () => {
        // Handle alert
        cy.get('#alertbtn').click()
        cy.on('window:alert', (text) => {
            expect(text).to.contains('Hello')
        })
    })

    it('should test confirm alert', () => {
        // Handle confirm alert
        cy.get('#confirmbtn').click()
        cy.on('window:confirm', (text) => {
            expect(text).to.contains('Hello')
        })
    })

    it('should test iFrame', () => {
        // Handle iframe
        cy.get('#courses-iframe').then(($iframe) => {
            const $body = $iframe.contents().find('body')
            cy.wrap($body).find('.navbar').should('exist')
        })
    })

    it('should test hide/show', () => {
        // Test hide/show functionality
        cy.get('#show-textbox').click()
        cy.get('#displayed-text').should('be.visible')
        cy.get('#hide-textbox').click()
        cy.get('#displayed-text').should('not.be.visible')
    })

    it('should test mouse hover', () => {
        // Test mouse hover
        cy.wait(2000)
        cy.get('.mouse-hover-content').invoke('show')
        cy.contains('Top').click()
        cy.url().should('include', 'top')
    })

    it('should test dynamic dropdown', () => {
        // Test dynamic dropdown
        cy.wait(2000)
        cy.get('.ui-autocomplete-input').type('In')
        cy.get('.ui-menu-item').each(($country) => {
            if ($country.text() === 'India') {
                cy.wrap($country).click()
            }
        })
        cy.get('#autocomplete').should('have.value', 'India')
    })
})
Running the Tests
Ensure you have followed the setup instructions.
Open the Cypress Test Runner:
npx cypress open
