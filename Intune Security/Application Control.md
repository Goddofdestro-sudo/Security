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

Configuration

Created App Configuration Policies for each supported Microsoft application.
Assigned the configuration policies to the same Azure AD groups used for the Application Protection Policies.
Verified deployment and successful application of settings on targeted devices.


2. Mobile Application Protection Policies
Two separate Application Protection Policies were implemented to support different security requirements for personal and corporate-managed devices.

Policy A – Personal (Unmanaged) Devices
Purpose: Protect organisational data on personal devices where the device itself is not enrolled or managed by Intune.
Key Security Controls

Block data transfer from managed applications to personal or third-party applications.
Restrict copy and paste actions outside managed applications.
Prevent data backup to personal cloud services.
Enforce application-level PIN and access controls.
Require corporate data to remain within approved Microsoft applications.

Assignment

Applied to all global users.
Utilised Intune device filters to automatically exclude managed devices.
Targeted only users accessing corporate data from unmanaged devices.

Outcome
Provided strong data-loss prevention controls while allowing users to access business resources from personal devices without requiring full device enrollment.

Policy B – Corporate (Managed) Devices
Purpose: Protect organisational data on Intune-managed corporate devices while maintaining business functionality.
Key Security Controls

Allowed copy and paste operations where business requirements existed.
Allowed screenshots and screen capture functionality.
Permitted data sharing with approved third-party applications.
Reduced restrictions compared to the unmanaged device policy to support operational requirements.

Risk Considerations
During implementation, limitations were identified where certain business-critical third-party applications could not be excluded or scoped separately using Application Protection Policies.
As a result:

Some restrictions were relaxed to maintain business operations.
Risks associated with data sharing were formally reviewed and accepted by the business.
Compensating controls remained in place through device management, compliance policies, and conditional access controls.

Assignment

Applied to all global users.
Utilised Intune device filters to automatically include managed devices.
Targeted only corporate-managed devices.


Outcome
The solution successfully strengthened the organisation's mobile security posture by protecting corporate data across both personal and corporate devices.
Benefits Achieved

Improved separation of corporate and personal data.
Reduced risk of accidental or intentional data leakage.
Enabled secure access to corporate resources from personal devices without requiring device enrollment.
Maintained user productivity while enforcing appropriate security controls.
Applied differentiated security controls based on device ownership and management state.
Balanced security requirements with business operational needs through documented risk acceptance where policy limitations existed.


Technologies Used

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
