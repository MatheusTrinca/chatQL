# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a real-time GraphQL chat application demonstrating subscriptions with Apollo Server, Apollo Client, and GraphQL-WS. The project has a client-server architecture with separate development servers.

## Development Commands

### Server (from /server directory)
- `npm start` - Start the GraphQL server with nodemon (auto-restart on changes)
- Server runs on port 9000 with GraphQL endpoint at `/graphql`

### Client (from /client directory)  
- `npm run dev` or `npm start` - Start Vite development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

### Database
- `node scripts/create-db.js` - Initialize SQLite database (run from /server directory)

## Architecture

### Server Architecture
- **GraphQL Server**: Apollo Server with Express middleware
- **Real-time**: WebSocket subscriptions using graphql-ws
- **Database**: SQLite with Knex.js query builder
- **Authentication**: JWT tokens with express-jwt middleware
- **Schema**: Simple chat schema with Query, Mutation, and Subscription types

### Client Architecture
- **Framework**: React 19 with Vite
- **GraphQL Client**: Apollo Client with subscription support
- **Styling**: Bulma CSS framework
- **Authentication**: JWT token stored in localStorage
- **Real-time**: GraphQL subscriptions over WebSocket

### Key Components
- **Apollo Client Setup**: `/client/src/lib/graphql/client.js` - Configures HTTP and WebSocket links with authentication
- **GraphQL Operations**: `/client/src/lib/graphql/queries.js` - Contains all queries, mutations, and subscriptions
- **Custom Hooks**: `/client/src/lib/graphql/hooks.js` - React hooks for GraphQL operations
- **Database Layer**: `/server/db/` - Database connection and data access functions

## Important Notes

### WebSocket Configuration Issue
The client WebSocket configuration in `client.js:31` points to `ws://localhost:4000/subscriptions` but the server runs on port 9000. This needs to be corrected to `ws://localhost:9000/graphql` for subscriptions to work.

### Authentication Flow
1. Login via POST `/login` endpoint returns JWT token
2. Client stores token in localStorage
3. Apollo Client adds `Authorization: Bearer <token>` header to requests
4. Server validates JWT and extracts user context

### Database Structure
- Uses better-sqlite3 with Knex.js
- Simple schema with users and messages tables
- Database file: `/server/data/db.sqlite3`

### Development Workflow
1. Start server: `cd server && npm start`
2. Start client: `cd client && npm run dev`
3. Server auto-restarts on .js/.graphql file changes
4. Client has hot module reload via Vite