# Principles Of Security

## CIA Triad

- Confidentiality: This element is the protection of data from unauthorized access and misuse. Organisations will always have some form of sensitive data stored on their systems. To provide confidentiality is to protect this data from parties that it is not intended for.

- Integrity: The CIA triad element of integrity is the condition where information is kept accurate and consistent unless authorized changes are made. It is possible for the information to change because of careless access and use, errors in the information system, or unauthorized access and use. In the CIA triad, integrity is maintained when the information remains unchanged during storage, transmission, and usage not involving modification to the information. Steps must be taken to ensure data cannot be altered by unauthorised people (for example, in a breach of confidentiality).

- Availability: In order for data to be useful, it must be available and accessible by the user. The main concern in the CIA triad is that the information should be available when authorised users need to access it.

## Principles of Privileges

- **PIM**: Privileged Identity Management, is used to translate a user's role within an organisation into an access role on a system
- **PAM**: Privileged Access Management, is the management of the privileges a system's access role has, amongst other things.

## Security models following the CIA Triad

- **The bell-La Padula Model:**The model works by granting access to pieces of data (called objects) on a strictly need to know basis. This model uses the rule "no write down, no read up".
  ![alt text](/JrPenetrationTester/PrinciplesOfSecurity/resources/bell-laPadulaModel.png)
- **Biba Model**: This model applies the rule to objects (data) and subjects (users) that can be summarised as "no write up, no read down". This rule means that subjects can create or write content to objects at or below their level but can only read the contents of objects above the subject's level.
  ![alt text](/JrPenetrationTester/PrinciplesOfSecurity/resources/bibaModel.png)
  The Biba model is used in organisations or situations where integrity is more important than confidentiality.

## Threat Modeling & Incident Response

**Threat modelling** is the process of reviewing, improving, and testing the security protocols in place in an organisation's information technology infrastructure and services.

The threat modelling process is very similar to a risk assessment made in workplaces for employees and customers. The principles all return to:

- Preparation
- Identification
- Mitigations
- Review

Framworks like PASTA & STRIDE
| Principle | Description |
|------------------------|-------------|
| **Spoofing** | This principle requires you to authenticate requests and users accessing a system. Spoofing involves a malicious party falsely identifying itself as another. Access keys (such as API keys) or signatures via encryption help remediate this threat. |
| **Tampering** | By providing anti-tampering measures to a system or application, you help provide integrity to the data. Data that is accessed must be kept integral and accurate. For example, shops use seals on food products. |
| **Repudiation** | This principle dictates the use of services such as logging of activity for a system or application to track. |
| **Information Disclosure** | Applications or services that handle information of multiple users need to be appropriately configured to only show information relevant to the owner. |
| **Denial of Service** | Applications and services use up system resources; these should have measures in place so that abuse of the application/service won't result in bringing the whole system down. |
| **Elevation of Privilege** | This is the worst-case scenario for an application or service. It means that a user was able to escalate their authorization to that of a higher level (e.g., an administrator). This often leads to further exploitation or information disclosure. |

Actions taken to resolve and remediate the threat are known as **Incident Response** (IR) and are a whole career path in cybersecurity.
![alt text](/JrPenetrationTester/PrinciplesOfSecurity/resources/IRClasiffication.png)
