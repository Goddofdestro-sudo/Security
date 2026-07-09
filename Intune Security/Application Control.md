**Objective**
Design and implement Microsoft Intune Application Protection Policies (MAM) to secure corporate data on mobile devices without requiring full device enrollment. The solution was designed to separate business and personal data, reduce the risk of data leakage, and maintain a positive end-user experience across both personally owned and corporate-managed iOS devices.

**Prerequisites
Before implementing the solution, the following activities were completed:**

-Identified all managed applications deployed within the environment.
-Reviewed application compatibility with Intune Application Protection Policies.
-Confirmed supported Microsoft applications that would be included within the MAM scope.
-Focused on Microsoft applications deployed across both personal and corporate-owned iOS devices.
-Validated user groups and assignment targets for policy deployment.


**Configuration Tasks**
**1. Application Configuration Policies**
Application Configuration Policies were created and assigned to all managed Microsoft applications that would be protected by the Application Protection Policies.
Purpose

**Standardize application settings.**
Ensure consistent user experience across managed applications.
Enable required application functionality before MAM enforcement.

**Configuration:**

Created App Configuration Policies for each supported Microsoft application.
<img width="1517" height="828" alt="image" src="https://github.com/user-attachments/assets/d27a78ac-ac66-4c62-a287-1e3e7960d277" />
<img width="1120" height="554" alt="image" src="https://github.com/user-attachments/assets/e7e99ae4-ffd6-46d1-8252-e5aafa84484b" />
Assigned the configuration policies to the same Azure AD groups used for the Application Protection Policies.
<img width="1065" height="122" alt="image" src="https://github.com/user-attachments/assets/d0b1ab4b-93a8-4fe0-a182-5c40f3f9a059" />

Verified deployment and successful application of settings on targeted devices.


2.** Mobile Application Protection Policies**
Two separate Application Protection Policies were implemented to support different security requirements for personal and corporate-managed devices.

Policy A – Personal (Unmanaged) Devices
Purpose: Protect organisational data on personal devices where the device itself is not enrolled or managed by Intune.

<img width="1290" height="39" alt="image" src="https://github.com/user-attachments/assets/825141d0-ec9c-48bc-b29d-e5256941938b" />

**Key Security Controls**

Block data transfer from managed applications to personal or third-party applications.
Restrict copy and paste actions outside managed applications.
Prevent data backup to personal cloud services.
Enforce application-level PIN and access controls.
Require corporate data to remain within approved Microsoft applications.
<img width="1100" height="881" alt="image" src="https://github.com/user-attachments/assets/710c9bd4-a568-4c89-9d46-feef82228d8b" />
<img width="614" height="626" alt="image" src="https://github.com/user-attachments/assets/539d7c6b-de23-4618-a087-c9795ab3afff" />
<img width="581" height="485" alt="image" src="https://github.com/user-attachments/assets/1289c1a9-420b-40c0-8c20-2c1ce47279cb" />

**Assignment**

Applied to all global users.
Utilised Intune device filters to automatically exclude managed devices.
Targeted only users accessing corporate data from unmanaged devices.
<img width="1264" height="608" alt="image" src="https://github.com/user-attachments/assets/59631d7d-dad6-4cd1-93ba-f4c0c4d711c4" />

**Outcome**
Provided strong data-loss prevention controls while allowing users to access business resources from personal devices without requiring full device enrollment.

<img width="368" height="800" alt="image" src="https://github.com/user-attachments/assets/43d74573-fc55-4cc1-a84f-9a81361701dc" />

Policy B – Corporate (Managed) Devices
Purpose: Protect organisational data on Intune-managed corporate devices while maintaining business functionality.
<img width="1256" height="45" alt="image" src="https://github.com/user-attachments/assets/a0488061-de6a-4460-8b15-be0f13320f14" />
**
Key Security Controls**

Allowed copy and paste operations where business requirements existed.
Allowed screenshots and screen capture functionality.
Permitted data sharing with approved third-party applications.
Reduced restrictions compared to the unmanaged device policy to support operational requirements.
<img width="933" height="788" alt="image" src="https://github.com/user-attachments/assets/aeed2141-0f71-4032-bae2-3d81a20e59a0" />
<img width="846" height="701" alt="image" src="https://github.com/user-attachments/assets/80704e64-f2a6-42b5-bbee-a86df5d42f5f" />
<img width="537" height="670" alt="image" src="https://github.com/user-attachments/assets/35cf4e90-1f49-4ea2-817b-2024e11f66e3" />
<img width="526" height="453" alt="image" src="https://github.com/user-attachments/assets/ad0b5cec-c873-4600-8318-56309f899f14" />

**Risk Considerations**
During implementation, limitations were identified where certain business-critical third-party applications could not be excluded or scoped separately using Application Protection Policies.
As a result:
Some restrictions were relaxed to maintain business operations.
Risks associated with data sharing were formally reviewed and accepted by the business.
Compensating controls remained in place through device management, compliance policies, and conditional access controls.

**Assignment**

Applied to all global users.
Utilised Intune device filters to automatically include managed devices.
Targeted only corporate-managed devices.

<img width="1166" height="534" alt="image" src="https://github.com/user-attachments/assets/0f8b34c1-5007-4359-8fbd-e37b160d254f" />

**Outcome**
The solution successfully strengthened the organisation's mobile security posture by protecting corporate data across both personal and corporate devices.
<img width="1280" height="136" alt="image" src="https://github.com/user-attachments/assets/57aca965-fcc7-4ebe-9da3-98fec14470d9" />

**Benefits Achieved**

Improved separation of corporate and personal data.
Reduced risk of accidental or intentional data leakage.
Enabled secure access to corporate resources from personal devices without requiring device enrollment.
Maintained user productivity while enforcing appropriate security controls.
Applied differentiated security controls based on device ownership and management state.
Balanced security requirements with business operational needs through documented risk acceptance where policy limitations existed.

**
Technologies Used**

Microsoft Intune
Microsoft Entra ID
Intune Application Protection Policies (MAM)
App Configuration Policies
Device Filters
Conditional Access
Microsoft 365 Mobile Applications


Lessons Learned

Device filters provide an effective method to differentiate managed and unmanaged devices without maintaining separate user groups.
Application Protection Policies offer strong data protection capabilities but may have limitations when interacting with certain third-party applications.
Business requirements and security controls must be balanced through formal risk assessment and risk acceptance processes where necessary.
Thorough testing across managed and unmanaged device scenarios is critical before production deployment.
