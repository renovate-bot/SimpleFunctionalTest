# SFT-tutorial


## Functional and acceptance testing using SimpleFunctionalTest

A quality is an obvious attribute or a property.

Quality assurance, is a way of ensuring the presence of qualities.

This could be done with tests.

In software, many kind of test ensure the quality:

* Unit Test ensure the correctness of an algorithm
* Integration Test ensure the component is integrated
* Benchmark ensure the performance.
* ...

The majority of them can be automatized.

Two kinds of tests ensure the compliance with customer/user needs:

* Acceptance test ensure a needs is developed
* Functional test ensure a functionality

These tests are bridges between human needs and software implementation: implementers are developers (in our case fluent java writers) and recipients are project managers or product owners (less comfortable with code).


SimpleFunctionalTest is a simple way to improve unit tests, to make them readable by non-developers.


# Methods

* The tutorial will used an ATM project as described in [What's in a story?](http://dannorth.net/whats-in-a-story/)
* All source used in this tutorial are available on [github](https://github.com/slezier/SFT-tutoriel); the different steps are versioned in the branches of the same name (step1,step2....).
* This project is build using maven 3.1 and JDK 1.6
* Basic knowledge of unit testing and JUnit 1.4 are needed.

# Step1: From an unit test to an functional test

This code can validate a functional test:

    public class WithdrawalsTest {

        private SessionDab atmSession;
        private int withdrawals;

        @Test
        public void testWithdrawals() {
            Bank bank = new Bank();
            User user = new User();
            Account account = bank.createAccount(user, 100);
            account.addValidCreditCard("1234");
            Atm atm = bank.getAtm(1000);

            atmSession = atm.authenticate(user);
            withdrawals = atmSession.withdraw(20);

            assertEquals(withdrawals, 20);
            assertEquals(account.balance(), 80);
            assertTrue("Card not returned", atm.returnCard());
        }
    }

As it is not _human readable_, it can't be shared between project manager and developers.


##  R&eacute;daction d'un test humainement 'lisible'

 Premier fonctionnalit&eacute; &agrave; tester: un retrait bancaire classique.
 
 Ce _cas d'utilisation_ peut faire l'objet de diff&eacute;rents _Sc&eacute;narios_ de tests: Cas passant (retrait autoris&eacute;), cas alternatif (retrait refus&eacute;), cas en &eacute;chec (&eacute;chec de connexion &agrave; la banque).

 Une _classe_ Java sera cr&eacute;e par _cas d'utilisation_. 
 Chaque _tests unitaires_ de cette classe d&eacute;criront un _sc&eacute;nario_.
 
 Le test doit être &eacute;crit de la façon la plus 'humaine' et la moins 'informatique' possible.

 Dans notre cas la classe de test s'appelera : _RetraitBanquaire_.
 
 Le premier sc&eacute;nario ( _retraitAutoris&eacute;_ ) n'est compos&eacute; que d'appels de m&eacute;thodes non publique (_fixtures_) d&eacute;crivant le sc&eacute;nario.
 
 Ces _fixtures_ permettent d'interagir avec le logiciel.
 
 Des champs non publiques seront utilis&eacute;s pour enregistrer le contexte associ&eacute; au test.

package bancomat;


public class RetraitBancaire{

  @Test
  public void retraitAutoris(){
    soitUnUtilisateurAyantUnCompteCreditDe1000Euros();
    quandIlDemandeUnRetraitDe200Euros();
    alorsLeGuichetDistribue200Euros();
    leCompteEstAlorsCrditDe800Euros();
  }

  private int compte ;
  private void soitUnUtilisateurAyantUnCompteCreditDe1000Euros(){
    compte = 1000;
  }
  private void quandIlDemandeUnRetraitDe200Euros(){
	guichet.sEnregistre(utilisateur, pin);
  }
  private void alorsLeGuichetDistribue200Euros(){
  }
  private void leCompteEstAlorsCrditDe800Euros(){
  }


}
  
  

    /*
    Scenario 1: Account has sufficient funds
Given the account balance is \$100
 And the card is valid
 And the machine contains enough money
When the Account Holder requests \$20
Then the ATM should dispense \$20
 And the account balance should be \$80
 And the card should be returned

    Scenario 2: Account has insufficient funds
Given the account balance is \$10
 And the card is valid
 And the machine contains enough money
When the Account Holder requests \$20
Then the ATM should not dispense any money
 And the ATM should say there are insufficient funds
 And the account balance should be \$20
 And the card should be returned

Scenario 3: Card has been disabled
Given the card is disabled
When the Account Holder requests \$20
Then the ATM should retain the card
And the ATM should say the card has been retained
     */

  Ajouter dans votre descripteur de pr