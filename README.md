# Software Hub - CRUD Management Application

A beautiful, production-ready React application for managing software portfolios with hierarchical categories, search functionality, and multiple view modes.

## Features

- **Complete CRUD Operations**: Create, Read, Update, and Delete software entries
- **Hierarchical Categories**: Three-level category structure (Business > Tech High > Tech Low)
- **Advanced Search & Filtering**: Search by name, ID, or owner with status filtering
- **Dual View Modes**: Toggle between card and list views
- **Responsive Design**: Optimized for all device sizes
- **Collapsible Sidebar**: Space-efficient navigation
- **Professional UI**: Modern design with smooth animations

## Data Structure

### Software Table
- **software_name** (Primary Key): Unique identifier for the software
- **software_id**: Internal software ID
- **status**: One of 'standard', 'edge case', 'retired', 'internal'
- **tech_category_low**: Specific technology (e.g., React, Python)
- **product_owner**: Person responsible for the software
- **url**: Application URL
- **business_category**: Top-level business category
- **tech_category_high**: High-level technology category

### Category Table
- **business_category**: Top-level business grouping
- **tech_category_high**: Technology domain within business category
- **tech_category_low**: Specific technology within tech domain
- **tech_category_low_id**: Unique identifier for tech category
- **owner**: Category owner
- **organization**: Responsible organization

## Hierarchy Structure
```
Business Category (e.g., Web Development)
├── Tech Category High (e.g., Frontend)
│   ├── Tech Category Low (e.g., React)
│   ├── Tech Category Low (e.g., Vue.js)
│   └── Tech Category Low (e.g., Angular)
└── Tech Category High (e.g., Backend)
    ├── Tech Category Low (e.g., Node.js)
    ├── Tech Category Low (e.g., Python)
    └── Tech Category Low (e.g., Java)
```

## How to Run

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Start Development Server**
   ```bash
   npm run dev
   ```

3. **Build for Production**
   ```bash
   npm run build
   ```

4. **Preview Production Build**
   ```bash
   npm run preview
   ```

## Changing Backend Data (SQL Migration)

Currently, the application uses mock data. To integrate with a real database, follow these steps:

### 1. Database Schema

Create the following tables in your SQL database:

```sql
-- Categories table
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    business_category VARCHAR(255) NOT NULL,
    tech_category_high VARCHAR(255) NOT NULL,
    tech_category_low VARCHAR(255) NOT NULL,
    tech_category_low_id VARCHAR(255) UNIQUE NOT NULL,
    owner VARCHAR(255) NOT NULL,
    organization VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Software table
CREATE TABLE software (
    software_name VARCHAR(255) PRIMARY KEY,
    software_id VARCHAR(255) UNIQUE NOT NULL,
    status ENUM('standard', 'edge case', 'retired', 'internal') NOT NULL,
    tech_category_low VARCHAR(255) NOT NULL,
    product_owner VARCHAR(255) NOT NULL,
    url VARCHAR(500),
    business_category VARCHAR(255) NOT NULL,
    tech_category_high VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (tech_category_low) REFERENCES categories(tech_category_low)
);

-- Indexes for better performance
CREATE INDEX idx_software_status ON software(status);
CREATE INDEX idx_software_category ON software(business_category, tech_category_high, tech_category_low);
CREATE INDEX idx_software_owner ON software(product_owner);
```

### 2. API Integration

Replace the mock data in `src/data/mockData.ts` with API calls:

```typescript
// Example API service
export const apiService = {
  async getSoftware() {
    const response = await fetch('/api/software');
    return response.json();
  },
  
  async createSoftware(software: Software) {
    const response = await fetch('/api/software', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(software)
    });
    return response.json();
  },
  
  async updateSoftware(softwareName: string, software: Software) {
    const response = await fetch(`/api/software/${softwareName}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(software)
    });
    return response.json();
  },
  
  async deleteSoftware(softwareName: string) {
    const response = await fetch(`/api/software/${softwareName}`, {
      method: 'DELETE'
    });
    return response.json();
  },
  
  async getCategories() {
    const response = await fetch('/api/categories');
    return response.json();
  }
};
```

### 3. Sample Data Migration

Insert sample data:

```sql
-- Insert sample categories
INSERT INTO categories (business_category, tech_category_high, tech_category_low, tech_category_low_id, owner, organization) VALUES
('Web Development', 'Frontend', 'React', 'react-001', 'John Smith', 'Development Team'),
('Web Development', 'Frontend', 'Vue.js', 'vue-001', 'Jane Doe', 'Development Team'),
('Web Development', 'Backend', 'Node.js', 'node-001', 'Sarah Wilson', 'Backend Team'),
('Data & Analytics', 'Database', 'PostgreSQL', 'postgres-001', 'Tom Anderson', 'Data Team'),
('Infrastructure', 'Cloud', 'AWS', 'aws-001', 'Mark Thompson', 'DevOps Team');

-- Insert sample software
INSERT INTO software (software_name, software_id, status, tech_category_low, product_owner, url, business_category, tech_category_high) VALUES
('React Dashboard', 'RD-2024-001', 'standard', 'React', 'John Smith', 'https://dashboard.company.com', 'Web Development', 'Frontend'),
('API Gateway', 'AG-2024-003', 'standard', 'Node.js', 'Sarah Wilson', 'https://api.company.com', 'Web Development', 'Backend'),
('Data Warehouse', 'DW-2024-006', 'standard', 'PostgreSQL', 'Tom Anderson', 'https://warehouse.company.com', 'Data & Analytics', 'Database');
```

## Technology Stack

- **React 18** with TypeScript
- **Tailwind CSS** for styling
- **Lucide React** for icons
- **Vite** for build tooling
- **Modern ES6+** features

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

MIT License - feel free to use this project for commercial or personal purposes.