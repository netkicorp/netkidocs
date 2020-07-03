# TransactID Standard Attestations

* Status: Draft
* Date: 2020-06-24
* Distribution: Public
* Version: 1.0


## Overview


* Background: TransactID uses x.509 certificates with mapped data from the Distinguished Name (DN) fields. 

* Limitations: Each individual DN field (i.e. O, OU, Locality, etc) is limited to 64-bytes.

* IVMS Lexicon: In the spirit of the IVMS effort to standardize language and terminology, we have included IVMS-101 terminology for each attestation in use.

* Distinguished Name Mappings: Each individual component of a DN is repurposed to represent attestation data.

	* CN == ivmsElement - the IVMS-101 term for the given object
	* State/Province == ivmsContsraint - these are parameters that work with ivmsDataType
	* OrganizationalUnit == data - this field will contain the value of the attested identity element
	* Organization == tidDescriptor - this is intended as  a friendly-name for the ivmsElement
	* Locality == ivmsDataType - this field, in conjunction withivmsConstraint allow additional parameters to be included within respective attestations as required
	* Country == data2 - this field is left intentionally empty; in the event that data value within the primary data field (OU) ventures beyond the 64-byte limit, this field can be used as an overflow for the remaining data value

* Additional Certificates: Additional Attestations can be added to accommodate for additional verification elements as required.


|        Attestation       |    format   |   DN Field ->  |             Common Name             | Country |            Locality            |       Organization      | Organizational Unit |     State/Province     |
|:------------------------:|:-----------:|:--------------:|:-----------------------------------:|:-------:|:------------------------------:|:-----------------------:|:-------------------:|:----------------------:|
|  naturalPersonFirstName  |  firstname  |        S       |    naturalName.primaryIdentifier    |         |     naturalPersonNameType=     |      naturalFirst=      |       Dr Alice      |    ALIA\|BIRT\|MAID    |
|   naturalPersonLastName  |   lastname  |        A       |   naturalName.secondaryIdentifier   |         |     naturalPersonNameType=     |       naturalLast=      |        Smith        |    ALIA\|BIRT\|MAID    |
|      legalPersonName     |    entity   |                |  legalPersonName.primaryIdentifier  |         | legalPersonNameIdentifierType= |     legalPersonName=    |   VASP of America   |          LEGL          |
|    beneficiaryPerson*    | beneficiary |                |   benficiaryName.primaryIdentifier  |    U    |     naturalPersonNameType=     |    beneficiaryFirst=    |        Robert       |    ALIA\|BIRT\|MAID    |
|    beneficiaryPerson*    | beneficiary |                | beneficiaryName.secondaryIdentifier |    N    |     naturalPersonNameType=     |     beneficiaryLast=    |        Barnes       |    ALIA\|BIRT\|MAID    |
|         birthDate        |  yyyy-mm-dd |        M       |             dateOfBirth             |    U    |           dateInPast=          |        birthdate=       |      1970-09-01     |       YYYY-MM-DD       |
|        birthPlace        |      XX     |        P       |             placeOfBirth            |    S    |          countryCode=          |         country=        |          US         |           XX           |
|         address1         | 123 address |        L       |          geographicAddress          |    E    |        addressTypeCode=        |         address=        |   123 Street Addy   |    GEOG\|BIZZ \|HOME   |
|         address2         | 456 address |                |          geographicAddress2         |    D    |        addressTypeCode=        |         address=        |     456 Cont Ln     |    GEOG\|BIZZ \|HOME   |
|    countryOfResidence    |      XX     | E              |          countryOfResidence         |         |          countryCode=          |         country=        |          US         |           XX           |
|      issuingCountry      |      XX     |        D       |          nationalIdentifier         |    F    |     nationalIdentifierType=    |   nationalIdentifier=   |          US         | RAID\|MISC\|LEIX\|TXID |
| nationalIdentifierNumber |   12345678  |        T       |      nationalIdentifier.number      |    I    |             number=            |        docnumber=       |        123456       |          12345         |
|    nationalIdentifier    |  identifier |        A       |      nationalIdentifier.docType     |    E    |     nationalIdentifierType=    |         doctype=        |       passport      |      ARNU \| DRLC      |
|       accountNumber      |   12345678  |                |            accountNumber            |    L    |         accountNumber=         |      accountNumber=     |         1234        |                        |
|  customerIdentification  |   12345678  |                |        customerIdentification       |    D    |     customerIdentification=    | customerIdentification= |         1234        |                        |
|   registrationAuthority  |   12345678  |                |        registrationAuthority        |         |     registrationAuthority=     |  registrationAuthority= |         1234        |                        |
|         cert type        |    format   | Field Alias -> |             ivmsElement             |  data2  |          ivmsDataType          |      tidDescriptor      |   data (examples)   |     ivmsConstraint     |