stateless authentication means that the server does not store any session information about users. Instead, every request sent by the client contains all the necessary data to verify the user, usually in the form of a token (such as a JWT - JSON Web Token). This is different from stateful authentication, where the server keeps track of user sessions, storing information like session IDs. 


                          example:

let usersDB = {
    'john@example.com': { password: '12345', role: 'admin' },
    'jane@example.com': { password: 'password', role: 'user' }
};

function authenticate(email, password) {
    const user = usersDB[email];
    if (user && user.password === password) {
      
        return { token: 'fake-token-123', role: user.role };
    } else {
        return { error: 'Invalid credentials' };
    }
}


function authorize(token, roleRequired) {
   
    if (token && token.role === roleRequired) {
        return { authorized: true };
    } else {
        return { error: 'Unauthorized access' };
    }
}


function publicAPI() {
    console.log('Public API: Accessible by anyone.');
}


function privateAPI(token) {
    const auth = authorize(token, 'admin');
    if (auth.authorized) {
        console.log('Private API: Accessible by authorized admin.');
    } else {
        console.log(auth.error);
    }
}


let userToken = authenticate('john@example.com', '12345');
if (userToken.token) {
    console.log('User authenticated:', userToken);

    
    publicAPI();

    
    privateAPI(userToken);
} else {
    console.log(userToken.error);
}
