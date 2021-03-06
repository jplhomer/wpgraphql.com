schema {
  query: RootQuery
  mutation: RootMutation
}

# Avatars are profile images for users. WordPress by default uses the Gravatar service to host and fetch avatars from.
type Avatar {
  # URL for the default image or a default type. Accepts &#039;404&#039; (return a
  # 404 instead of a default image), &#039;retro&#039; (8bit),
  # &#039;monsterid&#039; (monster), &#039;wavatar&#039; (cartoon face),
  # &#039;indenticon&#039; (the &#039;quilt&#039;), &#039;mystery&#039;,
  # &#039;mm&#039;, or &#039;mysteryman&#039; (The Oyster Man), &#039;blank&#039;
  # (transparent GIF), or &#039;gravatar_default&#039; (the Gravatar logo).
  default: String

  # HTML attributes to insert in the IMG element. Is not sanitized.
  extraAttr: String

  # Whether to always show the default image, never the Gravatar.
  forceDefault: Boolean

  # Whether the avatar was successfully found.
  foundAvatar: Boolean

  # Height of the avatar image.
  height: Int

  # What rating to display avatars up to. Accepts &#039;G&#039;, &#039;PG&#039;,
  # &#039;R&#039;, &#039;X&#039;, and are judged in that order.
  rating: String

  # Type of url scheme to use. Typically HTTP vs. HTTPS.
  scheme: String

  # The size of the avatar in pixels. A value of 96 will match a 96px x 96px gravatar image.
  size: Int

  # URL for the gravatar image source.
  url: String

  # Width of the avatar image.
  width: Int
}

# What rating to display avatars up to. Accepts 'G', 'PG', 'R', 'X', and are
# judged in that order. Default is the value of the 'avatar_rating' option
enum AvatarRatingEnum {
  G
  PG
  R
  X
}

# A Comment object
type Comment implements Node {
  # User agent used to post the comment. This field is equivalent to
  # WP_Comment-&gt;comment_agent and the value matching the `comment_agent` column
  agent: String

  # The approval status of the comment. This field is equivalent to
  # WP_Comment-&gt;comment_approved and the value matching the `comment_approved`
  approved: String

  # The author of the comment
  author: CommentAuthorUnion

  # IP address for the author. This field is equivalent to
  # WP_Comment-&gt;comment_author_IP and the value matching the
  # `comment_author_IP` column in SQL.
  authorIp: String

  # A collection of comment objects
  children(after: String, first: Int, before: String, last: Int, where: CommentArgs): CommentsConnection

  # ID for the comment, unique among comments.
  commentId: Int

  # The object the comment was added to
  commentedOn: PostObjectUnion

  # Content of the comment. This field is equivalent to
  # WP_Comment-&gt;comment_content and the value matching the `comment_content`
  content: String

  # Date the comment was posted in local time. This field is equivalent to
  # WP_Comment-&gt;date and the value matching the `date` column in SQL.
  date: String

  # Date the comment was posted in GMT. This field is equivalent to
  # WP_Comment-&gt;date_gmt and the value matching the `date_gmt` column in SQL.
  dateGmt: String

  # The globally unique identifier for the user
  id: ID!

  # Karma value for the comment. This field is equivalent to
  # WP_Comment-&gt;comment_karma and the value matching the `comment_karma` column
  karma: Int

  # Parent comment of current comment. This field is equivalent to the WP_Comment
  # instance matching the WP_Comment-&gt;comment_parent ID.
  parent: Comment

  # Type of comment. This field is equivalent to WP_Comment-&gt;comment_type and
  # the value matching the `comment_type` column in SQL.
  type: String
}

input CommentArgs {
  # Comment author email address.
  authorEmail: String

  # Array of author IDs to include comments for.
  authorIn: [Int]

  # Array of author IDs to exclude comments for.
  authorNotIn: [Int]

  # Comment author URL.
  authorUrl: String

  # Array of comment IDs to include.
  commentIn: [Int]

  # Array of IDs of users whose unapproved comments will be returned by the 
  # 							query regardless of status.
  commentNotIn: [Int]

  # Include comments of a given type.
  commentType: String

  # Include comments from a given array of comment types.
  commentTypeIn: [String]

  # Exclude comments from a given array of comment types.
  commentTypeNotIn: String

  # Content object author ID to limit results by.
  contentAuthor: [Int]

  # Array of author IDs to retrieve comments for.
  contentAuthorIn: [Int]

  # Array of author IDs *not* to retrieve comments for.
  contentAuthorNotIn: [Int]

  # Limit results to those affiliated with a given content object 
  # 							ID.
  contentId: Int

  # Array of content object IDs to include affiliated comments 
  # 							for.
  contentIdIn: [Int]

  # Array of content object IDs to exclude affiliated comments 
  # 							for.
  contentIdNotIn: [Int]

  # Content object name to retrieve affiliated comments for.
  contentName: String

  # Content Object parent ID to retrieve affiliated comments for.
  contentParent: Int

  # Array of content object statuses to retrieve affiliated comments for. 
  # 							Pass 'any' to match any value.
  contentStatus: [PostStatusEnum]

  # Content object type or array of types to retrieve affiliated comments for. Pass 'any' to match any value.
  contentType: [PostTypeEnum]

  # Array of author IDs to include comments for.
  includeUnapproved: [Int]

  # Karma score to retrieve matching comments for.
  karma: Int
  order: CommentsOrder

  # Field to order the comments by.
  orderby: CommentsOrderby

  # Parent ID of comment to retrieve children of.
  parent: Int

  # Array of parent IDs of comments to retrieve children for.
  parentIn: [Int]

  # Array of parent IDs of comments *not* to retrieve children 
  # 							for.
  parentNotIn: [Int]

  # Search term(s) to retrieve matching comments for.
  search: String

  # Comment status to limit results by.
  status: String

  # Include comments for a specific user ID.
  userId: Int
}

# A Comment Author object
type CommentAuthor implements Node {
  # The email for the comment author
  email: String

  # The globally unique identifier for the Comment Author user
  id: ID!

  # The name for the comment author.
  name: String

  # The url the comment author.
  url: String
}

union CommentAuthorUnion = User | CommentAuthor

# A connection to a list of items.
type CommentsConnection {
  # Information to aid in pagination.
  pageInfo: PageInfo!

  # Information to aid in pagination
  edges: [CommentsEdge]

  # The nodes of the connection, without the edges
  nodes: [Comment]
}

# An edge in a connection
type CommentsEdge {
  # The item at the end of the edge
  node: Comment

  # A cursor for use in pagination
  cursor: String!
}

enum CommentsOrder {
  ASC
  DESC
}

enum CommentsOrderby {
  COMMENT_AGENT
  COMMENT_APPROVED
  COMMENT_AUTHOR
  COMMENT_AUTHOR_EMAIL
  COMMENT_AUTHOR_IP
  COMMENT_AUTHOR_URL
  COMMENT_CONTENT
  COMMENT_DATE
  COMMENT_DATE_GMT
  COMMENT_ID
  COMMENT_IN
  COMMENT_KARMA
  COMMENT_PARENT
  COMMENT_POST_ID
  COMMENT_TYPE
  USER_ID
}

input CreateUserInput {
  # A string that contains the plain text password for the user.
  password: String

  # A string that contains a URL-friendly name for the user. The default is the user's username.
  nicename: String

  # A string containing the user's URL for the user's web site.
  websiteUrl: String

  # A string containing the user's email address.
  email: String

  # A string that will be shown on the site. Defaults to user's username. It is
  # likely that you will want to change this, for both appearance and security
  # through obscurity (that is if you dont use and delete the default admin user).
  displayName: String

  # The user's nickname, defaults to the user's username.
  nickname: String

  # 	The user's first name.
  firstName: String

  # The user's last name.
  lastName: String

  # A string containing content about the user.
  description: String

  # A string for whether to enable the rich editor or not. False if not empty.
  richEditing: String

  # The date the user registered. Format is Y-m-d H:i:s.
  registered: String

  # An array of roles to be assigned to the user.
  roles: [String]

  # User's Jabber account.
  jabber: String

  # User's AOL IM account.
  aim: String

  # User's Yahoo IM account.
  yim: String

  # User's locale.
  locale: String

  # If true, this will revoke the users JWT secret. If false, this will unrevoke
  # the JWT secret AND issue a new one. To revoke, the user must have proper
  # capabilities to edit users JWT secrets.
  revokeJwtUserSecret: Boolean

  # If true, this will refresh the users JWT secret.
  refreshJwtUserSecret: Boolean

  # A string that contains the user's username for logging in.
  username: String!
  clientMutationId: String!
}

type CreateUserPayload {
  user: User
  clientMutationId: String!
}

input DeleteUserInput {
  # The ID of the user you want to delete
  id: ID!

  # Reassign posts and links to new User ID.
  reassignId: ID
  clientMutationId: String!
}

type DeleteUserPayload {
  # The ID of the user that you just deleted
  deletedId: ID

  # The user object for the user you are trying to delete
  user: User
  clientMutationId: String!
}

input LoginInput {
  # The username used for login. Typically a unique or email address depending on specific configuration
  username: String!

  # The plain-text password for the user logging in.
  password: String!
  clientMutationId: String!
}

type LoginPayload {
  # JWT Token that can be used in future requests for Authentication
  authToken: String

  # A JWT token that can be used in future requests to get a refreshed
  # jwtAuthToken. If the refresh token used in a request is revoked or otherwise
  # invalid, a valid Auth token will NOT be issued in the response headers.
  refreshToken: String

  # The user that was logged in
  user: User
  clientMutationId: String!
}

# An object with an ID
interface Node {
  # The id of the object
  id: ID!
}

# Information about pagination in a connection.
type PageInfo {
  # When paginating forwards, are there more items?
  hasNextPage: Boolean!

  # When paginating backwards, are there more items?
  hasPreviousPage: Boolean!

  # When paginating backwards, the cursor to continue.
  startCursor: String

  # When paginating forwards, the cursor to continue.
  endCursor: String
}

# An plugin object
type Plugin implements Node {
  # Name of the plugin author(s), may also be a company name.
  author: String

  # URI for the related author(s)/company website.
  authorUri: String

  # Description of the plugin.
  description: String
  id: ID!

  # Display name of the plugin.
  name: String

  # URI for the plugin website. This is useful for directing users for support requests etc.
  pluginUri: String

  # Current version of the plugin.
  version: String
}

# A connection to a list of items.
type PluginsConnection {
  # Information to aid in pagination.
  pageInfo: PageInfo!

  # Information to aid in pagination
  edges: [PluginsEdge]

  # The nodes of the connection, without the edges
  nodes: [Plugin]
}

# An edge in a connection
type PluginsEdge {
  # The item at the end of the edge
  node: Plugin

  # A cursor for use in pagination
  cursor: String!
}

union PostObjectUnion = 

# The status of the object.
enum PostStatusEnum {
  # Objects with the auto-draft status
  AUTO_DRAFT

  # Objects with the draft status
  DRAFT

  # Objects with the future status
  FUTURE

  # Objects with the inherit status
  INHERIT

  # Objects with the pending status
  PENDING

  # Objects with the private status
  PRIVATE

  # Objects with the publish status
  PUBLISH

  # Objects with the trash status
  TRASH
}

# Allowed Post Types
enum PostTypeEnum {

}

input RefreshJwtAuthTokenInput {
  # A valid, previously issued JWT refresh token. If valid a new Auth token will
  # be provided. If invalid, expired, revoked or otherwise invalid, a new
  # AuthToken will not be provided.
  jwtRefreshToken: String!
  clientMutationId: String!
}

type RefreshJwtAuthTokenPayload {
  # JWT Token that can be used in future requests for Authentication
  authToken: String
  clientMutationId: String!
}

# The root mutation
type RootMutation {
  createUser(input: CreateUserInput!): CreateUserPayload
  deleteUser(input: DeleteUserInput!): DeleteUserPayload
  login(input: LoginInput!): LoginPayload
  refreshJwtAuthToken(input: RefreshJwtAuthTokenInput!): RefreshJwtAuthTokenPayload
  updateSettings(input: UpdateSettingsInput!): UpdateSettingsPayload
  updateUser(input: UpdateUserInput!): UpdateUserPayload
}

type RootQuery {
  allSettings: Settings

  # Returns a Comment
  comment(id: ID!): Comment

  # A collection of comment objects
  comments(after: String, first: Int, before: String, last: Int, where: CommentArgs): CommentsConnection

  # Fetches an object given its ID
  node(
    # The ID of an object
    id: ID!
  ): Node

  # A WordPress plugin
  plugin(id: ID!): Plugin

  # A collection of plugins
  plugins(after: String, first: Int, before: String, last: Int): PluginsConnection

  # A Theme object
  theme(id: ID!): Theme

  # A collection of theme objects
  themes(after: String, first: Int, before: String, last: Int): ThemesConnection

  # Returns a user
  user(id: ID!): User

  # A collection of user objects
  users(after: String, first: Int, before: String, last: Int, where: UserArgs): UsersConnection

  # Returns the current user
  viewer: User
}

enum SearchColumnsEnum {
  EMAIL
  ID
  LOGIN
  NICENAME
  URL
}

# All of the registered settings
type Settings {

}

# A theme object
type Theme implements Node {
  # Name of the theme author(s), could also be a company name. This field is
  # equivalent to WP_Theme-&gt;get( &quot;Author&quot; ).
  author: String

  # URI for the author/company website. This field is equivalent to WP_Theme-&gt;get( &quot;AuthorURI&quot; ).
  authorUri: String

  # The description of the theme. This field is equivalent to WP_Theme-&gt;get( &quot;Description&quot; ).
  description: String
  id: ID!

  # Display name of the theme. This field is equivalent to WP_Theme-&gt;get( &quot;Name&quot; ).
  name: String

  # The URL of the screenshot for the theme. The screenshot is intended to give an
  # overview of what the theme looks like. This field is equivalent to
  # WP_Theme-&gt;get_screenshot().
  screenshot: String

  # The theme slug is used to internally match themes. Theme slugs can have
  # subdirectories like: my-theme/sub-theme. This field is equivalent to
  # WP_Theme-&gt;get_stylesheet().
  slug: String

  # URI for the author/company website. This field is equivalent to WP_Theme-&gt;get( &quot;Tags&quot; ).
  tags: [String]

  # A URI if the theme has a website associated with it. The Theme URI is handy
  # for directing users to a theme site for support etc. This field is equivalent
  # to WP_Theme-&gt;get( &quot;ThemeURI&quot; ).
  themeUri: String

  # The current version of the theme. This field is equivalent to WP_Theme-&gt;get( &quot;Version&quot; ).
  version: Float
}

# A connection to a list of items.
type ThemesConnection {
  # Information to aid in pagination.
  pageInfo: PageInfo!

  # Information to aid in pagination
  edges: [ThemesEdge]

  # The nodes of the connection, without the edges
  nodes: [Theme]
}

# An edge in a connection
type ThemesEdge {
  # The item at the end of the edge
  node: Theme

  # A cursor for use in pagination
  cursor: String!
}

input UpdateSettingsInput {
  clientMutationId: String!
}

type UpdateSettingsPayload {
  allSettings: Settings
  clientMutationId: String!
}

input UpdateUserInput {
  # The ID of the user
  id: ID!

  # A string that contains the plain text password for the user.
  password: String

  # A string that contains a URL-friendly name for the user. The default is the user's username.
  nicename: String

  # A string containing the user's URL for the user's web site.
  websiteUrl: String

  # A string containing the user's email address.
  email: String

  # A string that will be shown on the site. Defaults to user's username. It is
  # likely that you will want to change this, for both appearance and security
  # through obscurity (that is if you dont use and delete the default admin user).
  displayName: String

  # The user's nickname, defaults to the user's username.
  nickname: String

  # 	The user's first name.
  firstName: String

  # The user's last name.
  lastName: String

  # A string containing content about the user.
  description: String

  # A string for whether to enable the rich editor or not. False if not empty.
  richEditing: String

  # The date the user registered. Format is Y-m-d H:i:s.
  registered: String

  # An array of roles to be assigned to the user.
  roles: [String]

  # User's Jabber account.
  jabber: String

  # User's AOL IM account.
  aim: String

  # User's Yahoo IM account.
  yim: String

  # User's locale.
  locale: String

  # If true, this will revoke the users JWT secret. If false, this will unrevoke
  # the JWT secret AND issue a new one. To revoke, the user must have proper
  # capabilities to edit users JWT secrets.
  revokeJwtUserSecret: Boolean

  # If true, this will refresh the users JWT secret.
  refreshJwtUserSecret: Boolean
  clientMutationId: String!
}

type UpdateUserPayload {
  # The updated user
  user: User
  clientMutationId: String!
}

# A User object
type User implements Node {
  # Avatar object for user. The avatar object can be retrieved in different sizes by specifying the size argument.
  avatar(
    # The size attribute of the avatar field can be used to fetch avatars of
    # different sizes. The value corresponds to the dimension in pixels to fetch.
    # The default is 96 pixels.
    size: Int = 96

    # Whether to always show the default image, never the Gravatar. Default false
    forceDefault: Boolean
    rating: AvatarRatingEnum
  ): Avatar

  # User metadata option name. Usually it will be `wp_capabilities`.
  capKey: String

  # This field is the id of the user. The id of the user matches WP_User-&gt;ID
  # field and the value in the ID column for the `users` table in SQL.
  capabilities: [String]

  # A collection of comment objects
  comments(after: String, first: Int, before: String, last: Int, where: CommentArgs): CommentsConnection

  # Description of the user.
  description: String

  # Email of the user. This is equivalent to the WP_User-&gt;user_email property.
  email: String

  # A complete list of capabilities including capabilities inherited from a role.
  # This is equivalent to the array keys of WP_User-&gt;allcaps.
  extraCapabilities: [String]

  # First name of the user. This is equivalent to the WP_User-&gt;user_first_name property.
  firstName: String

  # The globally unique identifier for the user
  id: ID!

  # Whether the JWT User secret has been revoked. If the secret has been revoked,
  # auth tokens will not be issued until an admin, or user with proper
  # capabilities re-issues a secret for the user.
  isJwtAuthSecretRevoked: Boolean!

  # The expiration for the JWT Token for the user. If not set custom for the user,
  # it will use the default sitewide expiration setting
  jwtAuthExpiration: String

  # A JWT token that can be used in future requests for authentication/authorization
  jwtAuthToken: String

  # A JWT token that can be used in future requests to get a refreshed
  # jwtAuthToken. If the refresh token used in a request is revoked or otherwise
  # invalid, a valid Auth token will NOT be issued in the response headers.
  jwtRefreshToken: String

  # A unique secret tied to the users JWT token that can be revoked or refreshed.
  # Revoking the secret prevents JWT tokens from being issued to the user.
  # Refreshing the token invalidates previously issued tokens, but allows new
  # tokens to be issued.
  jwtUserSecret: String

  # Last name of the user. This is equivalent to the WP_User-&gt;user_last_name property.
  lastName: String

  # The preferred language locale set for the user. Value derived from get_user_locale().
  locale: String

  # Display name of the user. This is equivalent to the WP_User-&gt;dispaly_name property.
  name: String

  # The nicename for the user. This field is equivalent to WP_User-&gt;user_nicename
  nicename: String

  # Nickname of the user.
  nickname: String

  # The date the user registered or was created. The field follows a full ISO8601 date string format.
  registeredDate: String

  # A list of roles that the user has. Roles can be used for querying for certain
  # types of users, but should not be used in permissions checks.
  roles: [String]

  # The slug for the user. This field is equivalent to WP_User-&gt;user_nicename
  slug: String

  # A website url that is associated with the user.
  url: String

  # The Id of the user. Equivelant to WP_User-&gt;ID
  userId: Int

  # Username for the user. This field is equivalent to WP_User-&gt;user_login.
  username: String
}

input UserArgs {
  # Array of IDs of users whose unapproved comments will be returned by the query regardless of status.
  exclude: [Int]

  # Pass an array of post types to filter results to users who have published posts in those post types.
  hasPublishedPosts: [PostTypeEnum]

  # Array of comment IDs to include.
  include: [Int]

  # The user login.
  login: String

  # An array of logins to include. Users matching one of these logins will be included in results.
  loginIn: Int

  # An array of logins to exclude. Users matching one of these logins will not be included in results.
  loginNotIn: Int

  # The user nicename.
  nicename: String

  # An array of nicenames to include. Users matching one of these nicenames will be included in results.
  nicenameIn: [String]

  # An array of nicenames to exclude. Users matching one of these nicenames will not be included in results.
  nicenameNotIn: [String]

  # An array of role names that users must match to be included in results. Note
  # that this is an inclusive list: users must match *each* role.
  role: UserRoleEnum

  # An array of role names. Matched users must have at least one of these roles.
  roleIn: [UserRoleEnum]

  # An array of role names to exclude. Users matching one or more of these roles will not be included in results.
  roleNotIn: [UserRoleEnum]

  # Search keyword. Searches for possible string matches on columns. When
  # `searchColumns` is left empty, it tries to determine which column to search in
  # based on search string.
  search: String

  # Array of column names to be searched. Accepts 'ID', 'login', 'nicename', 'email', 'url'.
  searchColumns: [SearchColumnsEnum]
}

enum UserRoleEnum {
  ADMINISTRATOR
  AUTHOR
  CONTRIBUTOR
  CUSTOMER
  EDITOR
  SHOP_MANAGER
  SUBSCRIBER
}

# A connection to a list of items.
type UsersConnection {
  # Information to aid in pagination.
  pageInfo: PageInfo!

  # Information to aid in pagination
  edges: [UsersEdge]

  # The nodes of the connection, without the edges
  nodes: [User]
}

# An edge in a connection
type UsersEdge {
  # The item at the end of the edge
  node: User

  # A cursor for use in pagination
  cursor: String!
}
