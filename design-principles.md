# Recommended Design Principles
We strongly recommend that Apps store only the `user_id` from CUBID in their back-end databases and avoid storing any additional user data. Instead, Apps should query the minimum required dataset directly into the front end on an as-needed basis, for each authenticated user session. In some use cases, caching specific data for the duration of the session may improve performance without sacrificing security, especially for high-frequency data needs. Our APIs are designed to be fast and reliable, optimized for repeated queries. 

Following this approach helps reduce attack vectors and ensures the user's sensitive data remains as secure as possible:

1. **Minimized Attack Surface**: By storing only the `user_id` in your back-end database and querying additional data only as needed, you significantly reduce the amount of sensitive data stored in your system. This limits the potential impact of a data breach, as hackers would not have access to any personal or sensitive information stored in the database.

2. **Real-Time Data Access**: Querying data during each user session ensures that you're always working with the most up-to-date information. This is particularly useful since users may add to their identity within CUBID from time to time, which also updates their Score.

3. **Reduced Data Liability**: By not storing sensitive user data on your back-end, you reduce the legal and operational burden of managing and protecting such data. This can also help with compliance under data privacy regulations like GDPR, which mandate strict controls around storing and processing personal data.

4. **Enhanced Security**: Keeping sensitive data on CUBID’s end ensures it benefits from the security infrastructure of a dedicated service, rather than depending on individual App implementations, which may vary in security rigor. This helps to reduce the risk of sensitive information being exposed due to poorly implemented or maintained security practices.

5. **Scalability**: As CUBID APIs are designed to be responsive and reliable, querying the data during each session minimizes the need for complex back-end infrastructure, which can help simplify scaling your App.

6. **Reduced Maintenance and Development Overhead**: By leveraging CUBID's APIs instead of developing and maintaining your own back-end calls, you simplify your infrastructure and avoid the complexity of managing sensitive data in-house. This allows your team to focus on core functionality, reducing development time and ongoing maintenance costs, while ensuring you’re always accessing accurate and secure data.
